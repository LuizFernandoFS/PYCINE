<script>
    import { once, subscribe } from "svelte/internal";
    import App from "../../App.svelte";
    import Person from "../Person/Person.svelte";
    import UserList from "./UserList.svelte";
    import UserUpdate from "./UserUpdate.svelte";

    let cancelForm = () => { window.history.go(-1); }

    let cadastrou = false;
    let mensagem = false;
    let resposta = "";
    async function sendForm(e) {
        // envia o formulario no formato json
        let formData = new FormData(e.target);
        let data = Object.fromEntries(formData.entries());
        const res = await fetch("http://localhost:8000/user/create", {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(data),
        });
        const json = await res.json();
        resposta = JSON.stringify(json);
        e.target.reset();
        cadastrou = true;
        mensagem = true;
        setTimeout(() => {
            mensagem = false;
        }, 3000);
    }
</script>

<h2>Novo Usuário</h2>
{#if mensagem}
    <p>Cadastro realizado com sucesso!</p>
{/if}
{#if cadastrou === false}
    <form class="crud" on:submit|preventDefault={sendForm}>
        <input
            type="text"
            name="name"
            placeholder="User name"
            required
            autocomplete="off"
        />
        <input
            type="email"
            name="email"
            placeholder="E-mail"
            required
            autocomplete="off"
        />
        <input
            type="password"
            name="password"
            placeholder="password"
            required
            autocomplete="off"
        />
        <button on:click={sendForm} class="btnSalvar">Salvar</button>
    </form>
    
{/if}
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
        color: whitesmoke;
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