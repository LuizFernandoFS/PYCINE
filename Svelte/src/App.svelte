<script>
    import Movie from "./lib/Movie/Movie.svelte";
    import User from "./lib/User/User.svelte";
    import Person from "./lib/Person/Person.svelte";
    import UserList from "./lib/User/UserList.svelte";
    import Favorite from "./lib/Favorite/Favorite.svelte";
    let menu = 2;
</script>

<main class="container">
    <h2>CRUD PYCINE</h2>

    <ul class="nav">
        <li>
            <a href="/" on:click|preventDefault={() => (menu = 1)}
                >Listar Usuários</a
            >
            <ul>
                <li>
                    <a href="/" on:click|preventDefault={() => (menu = 11)}
                        >Novo</a
                    >
                </li>
            </ul>
        </li>
        <li>
            <a href="/" on:click|preventDefault={() => (menu = 2)}
                >Listar Filmes</a
            >
        </li>
        <li>
            <a href="/" on:click|preventDefault={() => (menu = 3)}
                >Buscar Artista</a
            >
        </li>
        <li>
            <a href="/" on:click|preventDefault={() => (menu = 4)}>Favoritos</a>
        </li>
    </ul>

    <div class="card">
        {#if menu === 1}
            <UserList />
        {:else if menu === 11}
            <User />
        {:else if menu === 2}
            <Movie />
        {:else if menu === 3}
            <Person />
        {:else if menu === 4}
            <Favorite />
        {:else}
            <p>invalid option</p>
        {/if}
    </div>
</main>

<style>
    ul {
        list-style: none;
        margin: 0;
        padding: 0;
    }

    ul li {
        display: inline-table;
        position: relative;
        background-color: rgb(19, 122, 153);
        border-radius: 8px;
    }

    ul li a {
        display: inline-block;
        padding: 10px;
        color: whitesmoke;
        text-decoration: none;
    }

    ul li ul {
        position: absolute;
        top: 100%;
        left: 0;
        z-index: 1;
        display: none;
    }

    ul li:hover ul {
        display: inline-block;
        padding: 3px;
        color: whitesmoke;
        text-decoration: none;
    }
    .container {
        text-align: left;
        margin: 20px;
        padding: 10px;
    }
</style>
