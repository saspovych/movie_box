<template>
	<div class="container">
		<div class="top_img">
			<img v-bind:src=movieSideImg alt="">
		</div>
		<div class="bottom_details">
			<h2 class="bottom_title">{{movieDetails.movieTitle}}</h2>
			<div> <span class="movie_keyword green_details" v-for="keyword in movieDetails.movieKeywords">{{keyword}}</span></div>
			<p class="movie_description">{{movieDetails.movieDescription}}</p>
			<div class="movie_genres green_details"><span v-for="genre in movieDetails.movieGenre"> {{ genre }} </span></div>
			<div><span v-for="company in movieDetails.movieProductionCompanies">{{company}}</span></div>
			<div class="movie_number_details-grid">
				<div class="movie_number_details">
					Original Release: <span class="numbers_details green_details"><br>{{movieDetails.movieReleaseDate}}</span>
				</div>
				<div class="movie_number_details">
					Runnig Time: <span class="numbers_details green_details"><br>{{movieDetails.movieRuntime}}</span>
				</div>
				<div class="movie_number_details">
					Box Office: <span class="numbers_details green_details"><br>{{movieDetails.movieBudget}}</span>
				</div>
				<div class="movie_number_details">
					Vote Average: <span class="numbers_details green_details"><br>{{movieDetails.movieVote}}</span>
				</div>
			</div>
			<div class="movie_actors_wrapper">
				<div v-for="actor in movieDetails.movieActors">
					<figure>
						<img width="150" height="250" v-bind:src=movieDetails.imgSrc+actor.img alt="">
						<figcaption>
							<span class="actor_name">{{actor.name}}</span><br>
							<span class="character_name">{{actor.character}}</span>
						</figcaption>
					</figure>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
			}
		},
		props: {
			movieDetails: {
				type: Object,
				required: true,
			},
			movieSideImg: {
				type: String,
				required:true,
			}
		}
	}
</script>

<style scoped>
	.container {
		width: 1000px;
		margin: 0 auto;
		display: flex;
		flex-direction: column;
		color: #BFC0C0;
		background-color: rgba(45, 49, 66, 0.7);
		backdrop-filter: blur(10px);
		border-radius: 20px;
		font-size: 20px;
	}
	.top_img {
		height: 500px;
	}
	.top_img img {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}
	.bottom_title {
		text-transform: uppercase;
		font-size: 56px;
		line-height: 56px;
		margin-bottom: 15px;
	}
	.movie_description {
		line-height: 22px;
		margin: 5px 0;
	}
	.movie_genres,
	.movie_keyword {
		text-transform: uppercase;
	}
	.movie_genres {
		margin-top: 45px;
	}
	.bottom_details {
		padding: 20px;
	}
	.green_details {
		font-size: 32px;
		font-weight: 800;
		color: #27fb6b;
	}
	.numbers_details {
		line-height: 28px;
	}
	.movie_number_details-grid {
		display: grid;
		grid-template-columns: 0.7fr 1fr;
		margin-top: 45px;
	}
	.movie_actors_wrapper {
		display: grid;
		grid-template-columns: repeat(6, 1fr);
		margin-top: 65px;
	}
	.actor_name {
		font-size: 18px;
	}
	.character_name {
		font-size: 15px;
		opacity: 0.7;
	}
</style>