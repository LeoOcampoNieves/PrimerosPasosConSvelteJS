<script>
    export let id;
    export let title;
    export let overview;
    export let cover;

    $: coverURL = `https://image.tmdb.org/t/p/w185_and_h278_bestv2${cover}`;
</script>

<div class="card">
    <img class="card-img-top" src={coverURL} alt="Card image cap">
    
    <div class="card-body">
        <h5 class="card-title">{title}</h5>
        <p class="card-text">{overview}</p>
        
        <div class="text-center">
            <button class="btn btn-primary">Like</button>
        </div>
    </div>
</div>