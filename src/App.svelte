<script>
	import MovieItem from './Movie/Item.svelte'

	const APIKEY = '29ed1d64cc3508c30f08131eb1860d99';
	const BASEURL = 'https://api.themoviedb.org/3';
	const APISETTINGS = `?api_key=${APIKEY}&language=es-MX`;

	//Obtengo las peliculas
	const movies = (async () => {
		const URL = `${BASEURL}/discover/movie${APISETTINGS}$sort_by=popularity.desc`;
		
		const response = await fetch(URL);

		return await response.json();
	})()
</script>

<svelte:head>
	<title>Movies SvelteJS</title>
	
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
</svelte:head>

<main class="container">
	<div class="row">
		{#await movies}
			<p>Cargando datos...</p>
		{:then data}
			{@debug data}

			{#each data.results as movie}
				<div class="col-12 col-md-6 col-lg-3 p-2">
					<MovieItem id={movie.id} title={movie.title} overview={movie.overview} cover={movie.poster_path} />
				</div>
			{/each}
		{/await}
	</div>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>