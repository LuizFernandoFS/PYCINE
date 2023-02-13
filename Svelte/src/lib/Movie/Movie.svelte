<script>
    let salvou = false;
    let id_if;
    let promise = getMovies();
    async function getMovies() {
        const res = await fetch(`http://localhost:8000/movies`);
        const text = await res.json();
        if (res.ok) {
            return text;
        } else {
            throw new Error(text);
        }
    }
    function handleClick() {
        promise = getMovies();
    }
    async function saveFavorite(id) {
        salvou = true;
        id_if = id;
        const res = await fetch(`http://localhost:8000/favorite/create`, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify({ user_id: 1, movie_id: id }),
        });
        const text = await res.json();
        if (res.ok) {
            return text;
        } else {
            throw new Error(text);
        }
    }
</script>
<button on:click={handleClick}> Recarregar... </button>

{#await promise}
<p>...waiting</p>
{:then movies}
<div class="container">
{#each movies as m}
<div class="card">
{#if salvou && id_if == m.id}
<p style="color: green;">Adicionado aos favoritos!</p>
{/if}
<!-- svelte-ignore a11y-missing-attribute -->
<img src={m.poster_path} />
<p class="title">{m.title}</p>
<p class="genres">{m.genres}</p>
<button on:click={() => saveFavorite(m.id)}>Favoritar</button>
</div>
{/each}
</div>
{:catch error}
<p style="color: red">{error.message}</p>
{/await}

<style>
    .container {
        display: flex;
        flex-wrap: wrap;
        justify-content: left;
        margin-top: 20px;
    }
    .card {
        width: 200px;
        height: 500px;
        margin: 20px;
        box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
        transition: 0.3s;
        text-align: center;
    }
    .card:hover {
        box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
    }

    .card .title {
        font-weight: bold;
        margin-top: 10px;
    }
    .card .genres {
        margin-top: 5px;
        font-style: italic;
        color: gray;
    }
    .card button {
        margin-top: 10px;
        background-color: #ccc;
    }

</style>