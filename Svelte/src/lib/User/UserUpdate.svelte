<script>
    import App from "../../App.svelte";
import Person from "../Person/Person.svelte";
import User from "./User.svelte";
    import UserList from "./UserList.svelte";

    export let usuario;
    let cadastrou = false;
    let mensagem = false;
    let promise = getUser(usuario);

    async function getUser(usuario) {
        const res = await fetch(`http://localhost:8000/user/${usuario}`);
        const text = await res.json();
        if (res.ok) {
            return text;
        } else {
            throw new Error(text);
        }
    }

    async function sendForm(e) {
        // envia o formulario no formato json
        let formData = new FormData(e.target);
        let data = Object.fromEntries(formData.entries());
        const res = await fetch("http://localhost:8000/user/update", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(data),
        });
        e.target.reset();
        cadastrou = true;
        mensagem = true;
        setTimeout(() => {
            mensagem = false;
        }, 3000);
    }
</script>

{#if mensagem}
    <p>Atualizado com sucesso!</p>
{/if}
{#await promise}
    <p>...waiting</p>
{:then user}
    {#if cadastrou === false}
        <h2>Editar Usuário</h2>
        <form class="crud" on:submit|preventDefault={sendForm}>
            <input type="hidden" name="id" value={user.id} />
            <input
                type="text"
                name="name"
                placeholder="User name"
                required
                autocomplete="off"
                value={user.name}
            />
            <input
                type="text"
                name="email"
                placeholder="Email"
                required
                autocomplete="off"
                value={user.email}
            />
            <input
                type="text"
                name="password"
                placeholder="password"
                required
                autocomplete="off"
                value={user.password}
            />
            <button on:click={sendForm} class="btnSalvar">Salvar</button>
        </form>

    {/if}
{/await}
{#if cadastrou === true}
    <svelte:component this={UserList} />
{/if}

<style>

    form.crud {
        display: grid;
        grid-template-columns: 1fr;
        gap: 5px;
        row-gap: 10px;
    }

    p {
        color: green;
    }
    input {
        font-size: 22px;
        border-radius: 8px;
        text-align: center;
    }
    .btnSalvar {
        background-color: rgb(71, 187, 71);
        color: whitesmoke;
        border: 1px solid whitesmoke;
    }
</style>