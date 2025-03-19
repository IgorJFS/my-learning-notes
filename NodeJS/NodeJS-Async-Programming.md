# Node.js: Programação Assíncrona
Node.js brilha na programação assíncrona, essencial pra back-end (ex.: APIs, leitura de arquivos). Ele usa callbacks, Promises e async/await.
## Callbacks
Funções passadas como argumento que são chamadas quando uma tarefa termina.
Exemplo:
```javascript
const fs = require("fs");
fs.readFile("teste.txt", "utf8", (err, data) => {
  if (err) console.error("Erro:", err);
  else console.log("Conteúdo:", data);
});
```
Problema: Pode levar a "callback hell" (aninhamento excessivo).

## Promises
Promises são objetos que representam o resultado futuro de uma operação assíncrona.
Exemplo:
```javascript
const fs = require("fs").promises;
fs.readFile("teste.txt", "utf8")
  .then(data => console.log("Conteúdo:", data))
  .catch(err => console.error("Erro:", err));
```
Mais limpo que callbacks, com `.then` e `.catch`.

## Async/Await
Sintaxe moderna pra Promises, mais legível.
Exemplo:
```javascript
const fs = require("fs").promises;
async function lerArquivo() {
  try {
    const data = await fs.readFile("teste.txt", "utf8");
    console.log("Conteúdo:", data);
  } catch (err) {
    console.error("Erro:", err);
  }
}
lerArquivo();
```
`await` pausa a execução até a Promise resolver, mas só dentro de funções `async`.

## Exemplo Prático: API com Express
```javascript
const express = require("express");
const app = express();
app.get("/dados", async (req, res) => {
  try {
    const data = await fs.readFile("dados.txt", "utf8");
    res.send(data);
  } catch (err) {
    res.status(500).send("Erro no servidor");
  }
});
app.listen(3000, () => console.log("Rodando na porta 3000"));
```
Lê um arquivo e envia como resposta.

