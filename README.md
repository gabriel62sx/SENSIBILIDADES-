
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Gerador de Sensibilidade</title>
  <style>
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background-color: #000;
      color: white;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
    }

    .container {
      background-color: #111;
      padding: 2rem;
      border-radius: 16px;
      box-shadow: 0 0 30px rgba(255,255,255,0.05);
      width: 90%;
      max-width: 400px;
    }

    h2 {
      text-align: center;
      margin-bottom: 0.5rem;
    }

    p {
      text-align: center;
      margin-bottom: 1.5rem;
      color: #aaa;
    }

    .buttons-container {
      display: flex;
      justify-content: space-between;
      margin-bottom: 1rem;
    }

    .buttons-container button {
      flex: 1;
      margin: 0 5px;
      padding: 10px;
      border-radius: 8px;
      border: 2px solid white;
      background-color: black;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }

    .buttons-container button.active {
      background-color: white;
      color: black;
    }

    select, .btn-gerar {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border-radius: 8px;
      border: none;
      font-weight: bold;
    }

    .btn-gerar {
      background-color: white;
      color: black;
      cursor: pointer;
    }

    .result {
      background-color: #222;
      padding: 1rem;
      border-radius: 8px;
      margin-top: 1rem;
    }

    #auxilio-opcoes {
      background-color: #222;
      padding: 1rem;
      border-radius: 8px;
    }

    #auxilio-opcoes label {
      display: block;
      margin: 8px 0;
    }

    .btn-injetar {
      background-color: white;
      color: black;
      width: 100%;
      margin-top: 10px;
      padding: 10px;
      border: none;
      border-radius: 8px;
      font-weight: bold;
      cursor: pointer;
    }

    .success-message {
      text-align: center;
      color: #4CAF50;
      margin-top: 10px;
      font-weight: bold;
    }

    .footer {
      text-align: center;
      margin-top: 1.5rem;
      font-size: 0.9rem;
      color: white;
    }

    .footer a {
      color: white;
      text-decoration: none;
      font-weight: bold;
    }

    .sensi-only {
      display: block;
    }

    .hidden {
      display: none;
    }

    .progress {
      background-color: #333;
      border-radius: 10px;
      height: 10px;
      margin-top: 10px;
      overflow: hidden;
    }

    #progress-bar {
      height: 100%;
      width: 0%;
      background-color: #4CAF50;
      transition: width 0.1s linear;
    }
  </style>
</head>
<body>
  <div class="container">
    <h2>Sensibilidade / Auxílio</h2>
    <p>Configure a sensibilidade para usar</p>

    <div class="buttons-container">
      <button class="active" onclick="toggleMode(this, 'sensi')">Sensi</button>
      <button onclick="toggleMode(this, 'auxilio')">Auxílio</button>
    </div>

    <div class="sensi-only" id="sensi-content">
      <select id="marca" onchange="atualizarModelos()">
        <option value="Motorola">Motorola</option>
        <option value="Samsung">Samsung</option>
        <option value="Xiaomi">Xiaomi</option>
      </select>

      <select id="modelo">
        <!-- Modelos serão atualizados dinamicamente -->
      </select>

      <button class="btn-gerar" onclick="gerar()">Gerar</button>

      <div class="result" id="resultado"></div>
    </div>

    <div id="auxilio-opcoes" class="hidden">
      <label><input type="checkbox"> Calibrar sensibilidade</label>
      <label><input type="checkbox"> Reduzir recuo</label>
      <label><input type="checkbox"> Aumentar precisão</label>
      <label><input type="checkbox"> Retirar input lag</label>
      <button class="btn-injetar" onclick="injetar()">Injetar</button>
      <div id="progress-container" class="progress hidden">
        <div id="progress-bar"></div>
      </div>
      <div id="mensagem-injetar" class="success-message"></div>
    </div>

    <div class="footer">
      Desenvolvido por <a href="https://instagram.com/thomas_sheby82">@thomas_sheby82</a>
    </div>
  </div>

  <script>
    function toggleMode(button, mode) {
      const buttons = button.parentElement.querySelectorAll("button");
      buttons.forEach(btn => btn.classList.remove("active"));
      button.classList.add("active");

      document.getElementById("sensi-content").classList.toggle("hidden", mode !== "sensi");
      document.getElementById("auxilio-opcoes").classList.toggle("hidden", mode !== "auxilio");
    }

    function gerarNumeroAleatorio(min, max) {
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    function gerar() {
      const modelo = document.getElementById("modelo").value;

      const geral = gerarNumeroAleatorio(100, 200);
      const ponto = gerarNumeroAleatorio(100, 200);
      const mira2x = gerarNumeroAleatorio(100, 200);
      const mira4x = gerarNumeroAleatorio(100, 200);
      const botao = gerarNumeroAleatorio(40, 70);

      document.getElementById("resultado").innerHTML = `
        <p>Modelo: ${modelo}</p>
        <p>Geral: ${geral}</p>
        <p>Ponto Vermelho: ${ponto}</p>
        <p>Mira 2x: ${mira2x}</p>
        <p>Mira 4x: ${mira4x}</p>
        <p>Tamanho do Botão: ${botao}</p>
        <div class="success-message">Sucesso 🎁</div>
      `;
    }

    function injetar() {
      const bar = document.getElementById("progress-bar");
      const container = document.getElementById("progress-container");
      const mensagem = document.getElementById("mensagem-injetar");

      mensagem.innerText = "";
      bar.style.width = "0%";
      container.classList.remove("hidden");

      let progress = 0;
      const interval = setInterval(() => {
        if (progress >= 100) {
          clearInterval(interval);
          container.classList.add("hidden");
          mensagem.innerText = "Injetado com sucesso!";
        } else {
          progress += 2;
          bar.style.width = progress + "%";
        }
      }, 50);
    }

    function atualizarModelos() {
      const marca = document.getElementById("marca").value;
      const modeloSelect = document.getElementById("modelo");

      // Limpar modelos existentes
      modeloSelect.innerHTML = "";

      let modelos = [];

      // Definir modelos de acordo com a marca escolhida
      if (marca === "Motorola") {
        modelos = [
          "G8 Power", "Moto G10", "Moto G60", "Edge 20", "Edge Plus", "One Fusion", "Moto E7", "Moto G100"
        ];
      } else if (marca === "Samsung") {
        modelos = [
          "Galaxy S21", "Galaxy A51", "Galaxy A72", "Galaxy Z Fold 3", "Galaxy Note 20", "Galaxy S20 FE",
          "Galaxy A31", "Galaxy M32", "Galaxy S22 Ultra"
        ];
      } else if (marca === "Xiaomi") {
        modelos = [
          "Redmi Note 9", "Mi 11", "Poco X3", "Redmi Note 10", "Mi 10", "Poco F3", "Redmi 9", "Xiaomi 12"
        ];
      }

      // Adicionar modelos ao select
      modelos.forEach(modelo => {
        const option = document.createElement("option");
        option.textContent = modelo;
        modeloSelect.appendChild(option);
      });
    }

    // Chamar a função ao carregar a página para garantir que a lista esteja atualizada
    window.onload = atualizarModelos;
  </script>
</body>
</html>
