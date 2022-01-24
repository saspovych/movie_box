<template>
	<div class="app" v-bind:style="{background: 'url(' + aboutMovie.imgSrc+moviePoster + ') center center no-repeat fixed'}">
		<search-box @searchMovie="searchMovie" v-model:title=movieTitle></search-box>
		<info-block v-bind:movieDetails=aboutMovie v-bind:movieSideImg=aboutMovie.imgSrc+moviePoster></info-block>
	</div>
</template>

<script>
	import InfoBlock from "@/components/InfoBlock";
	import SearchBox from "@/components/SearchBox";
	export default {
		components: {
			InfoBlock, SearchBox
		},
		data() {
			return {
				apiKey: '6184b92d05b245414327a635a0e85149',
				movieId: '',
				aboutMovie: {
					movieTitle: 'Inception',
					movieKeywords: [],
					movieDescription: '',
					movieGenre: [],
					movieReleaseDate: '',
					movieActors: [],
					movieProductionCompanies: [],
					movieBudget: 0,
					movieRuntime: 0,
					movieVote: 0,
					imgSrc: 'https://image.tmdb.org/t/p/original',
				},

				moviePoster: '',
			}
		},
		methods: {
			async getInfoAboutMovie() {
				let fetchQuery = await fetch(`https://api.themoviedb.org/3/movie/${this.movieId}?api_key=${this.apiKey}`);
				fetchQuery = await fetchQuery.json();

				for(let company in fetchQuery.production_companies) {
					this.aboutMovie.movieProductionCompanies.push(fetchQuery.production_companies[company].name + " ");
				}
				this.aboutMovie.movieTitle = fetchQuery.original_title;
				this.aboutMovie.movieBudget = this.budgetConvert(fetchQuery.budget);
				this.aboutMovie.movieRuntime = fetchQuery.runtime + " mins";
				this.aboutMovie.movieVote = fetchQuery.vote_average + " / 10";
				this.movieId = fetchQuery.id;
				this.aboutMovie.movieDescription = fetchQuery.overview;
				this.aboutMovie.movieReleaseDate = fetchQuery.release_date;
				for(let genre in fetchQuery.genres) {
					this.aboutMovie.movieGenre.push(fetchQuery.genres[genre].name + " ");
				}

				let keywordsQuery = await fetch(`https://api.themoviedb.org/3/movie/${this.movieId}/keywords?api_key=${this.apiKey}`);
				keywordsQuery = await keywordsQuery.json();
				for(let i = 0; i < 3; i++) {
					this.aboutMovie.movieKeywords.push(keywordsQuery.keywords[i].name + " ")
				}
			},
			budgetConvert(budget) {
				console.log(typeof(budget))
				let temp = budget.toString();
				temp = temp.split('').reverse().join('');
				console.log(typeof(temp))
				for(let i = 0; i < temp.length; i++) {
					if ((i + 1) % 4 === 0) {
						temp = temp.slice(0, i) + "," + temp.slice(i);
					}
				}
				return "$" + temp.split('').reverse().join('');;
			},
			async aboutActors() {
				this.aboutMovie.movieActors = [];

				let actorsQuery = await fetch(`https://api.themoviedb.org/3/movie/${this.movieId}/credits?api_key=${this.apiKey}`);
				actorsQuery = await actorsQuery.json();
				for(let i = 0; i < 6; i++) {
					this.aboutMovie.movieActors.push({
						name: actorsQuery.cast[i].name,
						character: actorsQuery.cast[i].character,
						img: actorsQuery.cast[i].profile_path
					})
				}
				this.aboutMovie.movieActors = JSON.parse(JSON.stringify(this.aboutMovie.movieActors));
			},
			async getPoster() {
				let posterQuery = await fetch(`https://api.themoviedb.org/3/movie/${this.movieId}/images?api_key=${this.apiKey}`);
				posterQuery = await posterQuery.json();
				this.moviePoster = posterQuery.posters[0].file_path;
			},
			async searchMovie(title) {
				this.aboutMovie.movieTitle = title;
				this.aboutMovie.movieGenre = [];
				this.aboutMovie.movieProductionCompanies = [];
				this.aboutMovie.movieKeywords = [];

				let movieIdQuery = await fetch(`https://api.themoviedb.org/3/search/movie?api_key=${this.apiKey}&append_to_response=videos&query=${this.aboutMovie.movieTitle}`);
				movieIdQuery = await movieIdQuery.json();
				this.movieId = movieIdQuery.results[0].id;
				await this.getInfoAboutMovie();
				await this.getPoster();
				await this.aboutActors();
			}
		},
		mounted() {
			this.searchMovie(this.aboutMovie.movieTitle);
		}
	}
</script>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Oswald:wght@200;300;400;500;600;700&display=swap');
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
	}
	body {
		font-family: 'Oswald', sans-serif;
	}
	.app {
		padding: 75px 0;
	}
</style>