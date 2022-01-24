<template>
	<div class="search_box">
		<input @change="searchMovie" id="movieSearcher" v-model=title type="text" placeholder="Enter the movie title">
	</div>
</template>

<script>
	export default {
		data() {
			return {

			}
		},
		props: {
			title: {
				type: String,
				required: true,
			}
		},
		methods: {
			searchMovie() {
				if (this.title == "") {
					this.$emit("searchMovie", "Inception");
				} else {
					this.$emit("searchMovie", this.title);
				}
			}
		}
	}
</script>

<style scoped>
	.search_box {
		width: 1000px;
		margin: 0 auto;
		padding: 5px 15px;
		background-color: rgba(45, 49, 66, 0.7);
		backdrop-filter: blur(10px);
		border-radius: 20px 20px 0 0;
	}
	input {
		font-family: 'Oswald', sans-serif;
		color: #BFC0C0;
		border: none;
		outline: none;
		background-color: transparent;
		width: 100%;
		padding-right: 15px;
		font-size: 28px;
	}
	input:focus {
		color: white;
	}
</style>