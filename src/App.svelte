<script>
	import MovieItem from './Movie/Item.svelte'
	import { fly } from 'svelte/transition';

	const APIKEY = '29ed1d64cc3508c30f08131eb1860d99';
	const BASEURL = 'https://api.themoviedb.org/3';
	const APISETTINGS = `?api_key=${APIKEY}&language=es-MX`;

	//Obtengo las peliculas
	const movies = (async () => {
		const URL = `${BASEURL}/discover/movie${APISETTINGS}$sort_by=popularity.desc`;
		
		const response = await fetch(URL);

		return await response.json();
	})()

	let likedMovies = [];

	function toggleLike(event) {
		const movie = event.detail;

		let index = likedMovies.findIndex(m => m.id === movie.id);

		if(index >= 0) {
			likedMovies.splice(index, 1);
		} else {
			likedMovies.push(movie);
		}

		likedMovies = likedMovies;
	}

	// Comprobamos que la pelicula, sea o no, agregada a favoritos
	$: like = (id) => {
		let index = likedMovies.findIndex(m => m.id === id);

		return index >= 0;
	}
</script>

<svelte:head>
	<title>Movies SvelteJS</title>
	
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
</svelte:head>

<main class="container">
	<div class="row">
		<div class="col-12 col-md-6 col-lg-8 border panel">
			<h1>Peliculas populares</h1>

			<div class="row">
				{#await movies}
					<div class="col-12">
						<p>Cargando peliculas...</p>
					</div>
				{:then data}
					{@debug data}

					{#each data.results as movie}
						<div class="col-12 col-md-6 col-lg-4 p-2">
							<MovieItem like={like(movie.id)} id={movie.id} title={movie.title} overview={movie.overview} cover={movie.poster_path} on:onToggleLike={toggleLike} />
						</div>
					{/each}
				{/await}
			</div>
		</div>

		<div class="col-12 col-md-6 col-lg-4 border panel">
			<h2>Peliculas favoritas</h2>

			<div class="row">
				{#if likedMovies.length}
					{#each likedMovies as movie, i (movie.id)}
						<div in:fly="{{duration: 200, y: 20}}" out:fly="{{duration: 800, y: -20}}" class="col-12 col-md-6 col-lg-4 p-2">
							<MovieItem like={like(movie.id)} id={movie.id} title={movie.title} overview="" cover={movie.cover} on:onToggleLike={toggleLike} />
						</div>
					{/each}
				{:else}
					<div class="col-12">
						<p>No tienes peliculas favoritas</p>
					</div>
				{/if}
			</div>
		</div>
	</div>
</main>

<style>
	.panel {
		height: 100vh;
		overflow: auto;
	}

	main {
		text-align: center;
		padding: 0;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>