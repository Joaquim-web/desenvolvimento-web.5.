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
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
      background-color: #f0f2f5;
      margin: 0;
    }

    .pokedex {
      background: white;
      padding: 24px;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      width: 100%;
      max-width: 320px;
      text-align: center;
    }

    .search-box {
      display: flex;
      gap: 8px;
      margin-bottom: 20px;
    }

    input {
      flex: 1;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 6px;
      outline: none;
    }

    button {
      padding: 10px 16px;
      background-color: #ef5350;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:hover {
      background-color: #e53935;
    }

    .pokemon-card {
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .pokemon-card img {
      width: 120px;
      height: 120px;
    }

    .types {
      display: flex;
      gap: 6px;
      margin-top: 8px;
    }

    .type-badge {
      background: #eee;
      padding: 4px 8px;
      border-radius: 4px;
      font-size: 0.85rem;
      text-transform: capitalize;
    }

    .error {
      color: #d32f2f;
    }
  </style>
</head>
<body>

  <div class="pokedex">
    <h2>Mini Pokédex</h2>
    <div class="search-box">
      <input type="text" id="pokemonInput" placeholder="Digite o nome do Pokémon...">
      <button id="searchBtn">Buscar</button>
    </div>
    <div id="content"></div>
  </div>

  <script src="script.js"></script>
</body>
</html>
