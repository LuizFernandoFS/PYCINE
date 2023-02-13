<script>
    let formatDate = (date) => {
      var d = new Date(date),
        month = "" + (d.getMonth() + 1),
        day = "" + d.getDate(),
        year = d.getFullYear();
      if (month.length < 2) month = "0" + month;
      if (day.length < 2) day = "0" + day;
      return [day, month, year].join("/");
    };
  
    let nome = null;
    let person = null;
    let message = null;
    let error = false;
    async function getPerson() {
      const res = await fetch(`http://localhost:8000/artist/name/${nome}`);
      const data = await res.json();
      if (res.ok) {
        person = data;
        message = null;
      } else {
        person = null;
        message = `ID ${nome} ${res.statusText}`;
      }
      if (person === null) {
        error = true;
      }
    }
  </script>
  
  <div class="search-container">
    <input
      bind:value={nome}
      type="text"
      placeholder="Digite o nome do ator:"
    />
    <button on:click={getPerson} style="border: solid 1px black; margin-left: 5px;"> 
        Buscar 
    </button>
  </div>
  
  <div class="card-container">
    {#if person !== null}
      <div class="card">
        <img
          src="https://image.tmdb.org/t/p/w185{person.imagem}"
          alt="imagem do artista"
          width="300"
          height="300"
        />
        <ul>
          <li><h3>Nome: {person.nome}</h3></li>
          <li><h3>Popularidade: {person.popularidade}</h3></li>
          <li><h3>Data de Nascimento: {formatDate(person.dataNascimento)}</h3></li>
          <li><h3>Biografia: {person.biografia}</h3></li>
        </ul>
      </div>
    {:else if error}
      <h3 style="color:red">Erro: Artista não encontrado!</h3>
    {/if}
  </div>
  
  <style>
    .search-container {
      display: flex;
      margin-right: 15px;
      justify-content: left;
      align-items: left;
      margin-bottom: 10px;
    }
  
    .card-container {
      display: flex;
      justify-content: center;
    }
  
    .card {
      display: grid;
      grid-template-columns: 1fr 1fr 1fr;
      width: 1500px;
      padding: 20px;
      border: 1px solid #ccc;
      border-radius: 10px;
    }
  
    input {
      font-size: 25px;
      border-radius: 3px;
    }

</style>
  