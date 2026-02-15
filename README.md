# Congrega-o-Mineiros-32623
<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Congregação Mineiros do Tietê</title>

<style>
body {
  font-family: Arial, sans-serif;
  background: #f4f6f9;
  margin: 0;
  padding: 0;
}

header {
  background: #2b579a;
  color: white;
  padding: 15px;
  text-align: center;
  font-size: 20px;
}

.container {
  padding: 15px;
}

button {
  padding: 10px;
  margin: 5px 0;
  width: 100%;
  border: none;
  border-radius: 5px;
  font-size: 16px;
}

.green { background: #28a745; color: white; }
.red { background: #dc3545; color: white; }
.yellow { background: #ffc107; }
.blue { background: #2b579a; color: white; }

input, select {
  width: 100%;
  padding: 8px;
  margin: 5px 0;
}

.card {
  background: white;
  padding: 10px;
  margin: 10px 0;
  border-radius: 5px;
}
</style>
</head>

<body>

<header>
Congregação Mineiros do Tietê
</header>

<div class="container">

<h3>Adicionar Membro</h3>

<input type="text" id="nome" placeholder="Nome do membro">

<select id="categoria">
  <option>Publicador</option>
  <option>Pioneiro Auxiliar</option>
  <option>Pioneiro Regular</option>
</select>

<button class="blue" onclick="adicionarMembro()">Adicionar</button>

<hr>

<h3>Lista de Membros</h3>

<div id="lista"></div>

</div>

<script>

let membros = JSON.parse(localStorage.getItem("membros")) || [];

function salvar() {
  localStorage.setItem("membros", JSON.stringify(membros));
  render();
}

function adicionarMembro() {
  const nome = document.getElementById("nome").value;
  const categoria = document.getElementById("categoria").value;

  if (!nome) return alert("Digite o nome");

  membros.push({
    nome,
    categoria,
    estudos: {},
    presencas: {}
  });

  document.getElementById("nome").value = "";
  salvar();
}

function registrarPresenca(index, status) {
  const hoje = new Date().toISOString().split("T")[0];
  membros[index].presencas[hoje] = status;
  salvar();
}

function render() {
  const lista = document.getElementById("lista");
  lista.innerHTML = "";

  membros.forEach((m, i) => {
    lista.innerHTML += `
      <div class="card">
        <b>${m.nome}</b><br>
        ${m.categoria}<br><br>

        <button class="green" onclick="registrarPresenca(${i}, 'Participou')">Participou</button>
        <button class="red" onclick="registrarPresenca(${i}, 'Não participou')">Não participou</button>
        <button class="yellow" onclick="registrarPresenca(${i}, 'Irregular')">Irregular</button>
      </div>
    `;
  });
}

render();

</script>

</body>
</html>
