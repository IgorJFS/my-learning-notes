# Node.js: Construindo REST APIs
REST (Representational State Transfer) é um estilo de arquitetura pra criar APIs que usam métodos HTTP (GET, POST, PUT, DELETE) pra manipular recursos. Node.js, com Express, é perfeito pra isso por ser leve e assíncrono.
## O que é uma REST API?
**Recursos**: Tudo é um "recurso" (ex.: usuários, produtos) identificado por URLs.

**Métodos HTTP**:
`GET`: Lê dados.

`POST`: Cria dados.

`PUT`: Atualiza dados.

`DELETE`: Remove dados.

**Respostas**: Geralmente em JSON.

## Exemplo Básico com Express
Crie uma API simples pra gerenciar uma lista de tarefas.
Instale o Express:
```bash
npm init -y
npm install express
```

Crie `api.js`:
```javascript
const express = require("express");
const app = express();
app.use(express.json()); // Pra parsing de JSON no body

let tarefas = ["Estudar Node.js", "Fazer café"];
// GET - Lista todas as tarefas
app.get("/tarefas", (req, res) => {
  res.json(tarefas);
});
// GET - Pega uma tarefa por ID
app.get("/tarefas/:id", (req, res) => {
  const id = parseInt(req.params.id);
  if (id >= 0 && id < tarefas.length) {
    res.json({ tarefa: tarefas[id] });
  } else {
    res.status(404).json({ erro: "Tarefa não encontrada" });
  }
});
// POST - Adiciona uma tarefa
app.post("/tarefas", (req, res) => {
  const novaTarefa = req.body.tarefa;
  if (novaTarefa) {
    tarefas.push(novaTarefa);
    res.status(201).json({ mensagem: "Tarefa adicionada", tarefas });
  } else {
    res.status(400).json({ erro: "Tarefa inválida" });
  }
});
// PUT - Atualiza uma tarefa
app.put("/tarefas/:id", (req, res) => {
  const id = parseInt(req.params.id);
  const novaTarefa = req.body.tarefa;
  if (id >= 0 && id < tarefas.length && novaTarefa) {
    tarefas[id] = novaTarefa;
    res.json({ mensagem: "Tarefa atualizada", tarefas });
  } else {
    res.status(400).json({ erro: "ID ou tarefa inválida" });
  }
});
// DELETE - Remove uma tarefa
app.delete("/tarefas/:id", (req, res) => {
  const id = parseInt(req.params.id);
  if (id >= 0 && id < tarefas.length) {
    tarefas.splice(id, 1);
    res.json({ mensagem: "Tarefa removida", tarefas });
  } else {
    res.status(404).json({ erro: "Tarefa não encontrada" });
  }
});
app.listen(3000, () => console.log("API rodando na porta 3000"));
```
Teste:

Rode com `node api.js`.

Use ferramentas como Postman ou `curl`:
`GET http://localhost:3000/tarefas\`

`POST http://localhost:3000/tarefas\` com body `{"tarefa": "Dormir"}`

`PUT http://localhost:3000/tarefas/0\` com body `{"tarefa": "Estudar mais"}`

`DELETE http://localhost:3000/tarefas/1\`

## Por que usar Node.js pra REST?
**Rápido**: O event loop lida com muitas requisições sem travar.

**JSON nativo**: JS trabalha bem com JSON, formato padrão de REST.

**Ecossistema**: Pacotes como `express-validator` (validação) e `cors` (cross-origin) facilitam.

## Dica
Use `nodemon` pra reiniciar o servidor automaticamente:
```bash
npm install -g nodemon
nodemon api.js
```
