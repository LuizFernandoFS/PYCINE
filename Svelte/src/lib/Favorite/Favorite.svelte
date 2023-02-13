<script>
    let promise = getMovies();
    async function getMovies() {
        const res = await fetch(`http://localhost:8000/favorite/movies/${1}`);
        const text = await res.json();
        if (res.ok) {
            return text;
        } else {
            throw new Error(text);
        }
    }

</script>

{#await promise}
    <p>Carregando...</p>
{:then movies}
    <div class="table">
        {#each movies as m}
            <div class="card">
                <!-- svelte-ignore a11y-missing-attribute -->
                <p><img src={m.poster_path} /></p>
                <p>{m.name}</p>
            </div>
        {/each}
    </div>
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}

<style>
    .table {
        display: flex;
        flex-wrap: wrap;
        justify-content:space-between;
        padding: 10px;
    }
    .card {
        width: 180px;
        height: 360px;
        background-color: #fff;
        box-shadow: 0px 0px 10px #ccc;
        margin-bottom: 20px;
        text-align: center;
        padding: 10px;
    }

</style>
