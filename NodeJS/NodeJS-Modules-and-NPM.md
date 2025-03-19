# Node.js: Módulos e NPM
Node.js organiza código em **módulos** e usa o **NPM** pra gerenciar dependências. Isso é essencial pra estruturar projetos back-end.
## Módulos
Node.js tem um sistema de módulos baseado em CommonJS. Cada arquivo é um módulo, e você importa/exporta funcionalidades com `require` e `module.exports`.
### Módulos Built-in
Exemplo com `fs` (file system):
```javascript
const fs = require("fs");
fs.writeFileSync("teste.txt", "Oi, Zackie!");
console.log(fs.readFileSync("teste.txt", "utf8")); // "Oi, Zackie!"
```
`fs`: Lê/escreve arquivos sem instalar nada.

### Módulos Customizados
Crie `math.js`:
```javascript
function soma(a, b) {
  return a + b;
}
module.exports = { soma };
```
Use em `main.js`:
```javascript
const math = require("./math");
console.log(math.soma(2, 3)); // 5
```
## NPM
NPM é o gerenciador de pacotes do Node.js. Pra começar um projeto:
`npm init`: Cria um `package.json` (metadados do projeto).

`npm install <pacote>`: Instala dependências.

### Exemplo com Express
```bash
npm init -y
npm install express
```
Crie `server.js`:
```javascript
const express = require("express");
const app = express();
app.get("/", (req, res) => {
  res.send("Oi, Zackie! Bem-vindo ao Express!");
});
app.listen(3000, () => console.log("Servidor na porta 3000"));
```
Rode com `node server.js` e acesse `http://localhost:3000\`.

### Scripts no package.json
Edite `package.json`:
```json
"scripts": {
  "start": "node server.js"
}
```
Use `npm start` pra rodar.

## Dica
Use `npm install -g <pacote>` pra instalar globalmente (ex.: `npm install -g nodemon` pra reiniciar o servidor automaticamente).

