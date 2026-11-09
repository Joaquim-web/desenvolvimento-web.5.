# desenvolvimento-web.5.
entregavel de desenvolvimento web semana 5

<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Mini Pokédex</title>

  <style>
    
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      box-sizing: border-box;
      background: linear-gradient(to bottom, #d32f2f 50%, #1565c0 50%);
    }

    /* Cartão da pokédex */
    .pokedex {
      background: white;
      width: 100%;
      max-width: 360px;
      border-radius: 16px;
      overflow: hidden;
      box-shadow: 0 10px 25px rgba(0, 0, 0, 0.4);
      border: 4px solid #222;
    }

    /* Topo vermelho com o título */
    .topo {
      background-color: #d32f2f;
      color: white;
      padding: 16px;
      text-align: center;
      border-bottom: 4px solid #222;
    }

    .topo h1 {
      margin: 0;
      font-size: 24px;
    }

    /* Parte da busca */
    .busca-area {
      padding: 16px;
    }

    .search-box {
      display: flex;
      gap: 8px;
    }

    input {
      flex: 1;
      padding: 10px;
      border: 2px solid #1565c0;
      border-radius: 6px;
      outline: none;
      font-size: 16px;
    }

    button {
      padding: 10px 16px;
      background-color: #1565c0;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 16px;
    }

    button:hover {
      background-color: #0d47a1;
    }

    /* Tela azul onde aparece o resultado */
    .tela {
      background-color: #1976d2;
      border-top: 4px solid #222;
      min-height: 280px;
      padding: 20px;
      text-align: center;
      color: white;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }

    .mensagem {
      margin: 0;
    }

    .error {
      background-color: #d32f2f;
      color: white;
      padding: 10px 16px;
      border-radius: 6px;
      font-weight: bold;
      margin: 0;
    }

    /* Resultado: nome em cima, depois imagem e tipos */
    .pokemon-card {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .pokemon-card h2 {
      text-transform: capitalize;
      background-color: #d32f2f;
      color: white;
      padding: 8px 24px;
      border-radius: 30px;
      border: 3px solid #222;
      margin: 0 0 16px 0;
      font-size: 26px;
    }

    /* Círculo vermelho e branco, tipo pokébola */
    .imagem-fundo {
      width: 170px;
      height: 170px;
      border-radius: 50%;
      background: linear-gradient(to bottom, #ef5350 50%, white 50%);
      border: 4px solid #222;
      display: flex;
      justify-content: center;
      align-items: center;
    }

    .pokemon-card img {
      width: 140px;
      height: 140px;
    }

    .numero {
      margin: 10px 0 0 0;
      font-weight: bold;
    }

    .types {
      display: flex;
      gap: 6px;
      margin-top: 10px;
    }

    .type-badge {
      background: white;
      color: #d32f2f;
      font-weight: bold;
      padding: 4px 14px;
      border-radius: 20px;
      font-size: 0.85rem;
      text-transform: capitalize;
    }
  </style>
</head>
<body>

  <div class="pokedex">

    <div class="topo">
      <h1>Mini Pokédex</h1>
    </div>

    <div class="busca-area">
      <form class="search-box" id="formulario">
        <input type="text" id="busca" placeholder="Ex: pikachu">
        <button type="submit">Buscar</button>
      </form>
    </div>

    <!-- Aqui aparecem os 4 estados: inicial, carregando, sucesso e erro -->
    <div class="tela" id="resultado">
      <p class="mensagem">Digite o nome de um Pokémon para começar.</p>
    </div>

  </div>

  <script>
    const formulario = document.getElementById("formulario");
    const campoBusca = document.getElementById("busca");
    const resultado = document.getElementById("resultado");

    // Estado 2: carregando
    function mostrarCarregando() {
      resultado.innerHTML = '<p class="mensagem">Carregando...</p>';
    }

    // Estado 3: sucesso (o nome aparece em cima)
    function mostrarPokemon(pokemon) {
      let tiposHtml = "";
      for (let i = 0; i < pokemon.types.length; i++) {
        tiposHtml += `<span class="type-badge">${pokemon.types[i].type.name}</span>`;
      }

      // se por algum motivo não tiver a imagem padrão, usa a artwork oficial
      let imagem = pokemon.sprites.front_default;
      if (!imagem) {
        imagem = pokemon.sprites.other["official-artwork"].front_default;
      }

      resultado.innerHTML = `
        <div class="pokemon-card">
          <h2>${pokemon.name}</h2>
          <div class="imagem-fundo">
            <img src="${imagem}" alt="Imagem do ${pokemon.name}">
          </div>
          <p class="numero">#${pokemon.id}</p>
          <div class="types">${tiposHtml}</div>
        </div>
      `;
    }

    // Estado 4: erro
    function mostrarErro(mensagem) {
      resultado.innerHTML = `<p class="error">${mensagem}</p>`;
    }

    // Busca o pokémon na API
    async function buscarPokemon(nome) {
      mostrarCarregando();

      try {
        const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${nome}`);

        // o fetch NÃO dá erro sozinho no 404, por isso checamos response.ok
        if (!response.ok) {
          if (response.status === 404) {
            mostrarErro("Pokémon não encontrado");
          } else {
            mostrarErro("Erro ao buscar o Pokémon. Tente de novo.");
          }
          return;
        }

        const dados = await response.json();
        console.log(dados); // pra você ver os dados no console (F12)
        mostrarPokemon(dados);
      } catch (erro) {
        // cai aqui se ficar sem internet ou se o navegador bloquear o pedido
        console.log("Erro:", erro);
        mostrarErro("Não foi possível conectar. Verifique sua internet.");
      }
    }

    // Quando clica em Buscar (ou aperta Enter)
    formulario.addEventListener("submit", function (evento) {
      evento.preventDefault();

      const nome = campoBusca.value.trim().toLowerCase();

      if (nome === "") {
        mostrarErro("Digite o nome de um Pokémon");
        return;
      }

      buscarPokemon(nome);
    });
  </script>

</body>
</html>
