# Middlewares no Node.js

Middlewares são funções intermediárias que processam requisições e respostas em aplicações Node.js, especialmente com frameworks como Express.js. Eles permitem modularizar e reutilizar lógicas comuns, como autenticação, logging, tratamento de erros, entre outros.

---

## 1. O que é um Middleware?

- Uma função que recebe os objetos `request` (req), `response` (res) e o próximo middleware (`next`).
- Pode modificar a requisição, resposta ou encerrar o ciclo de requisição.
- Se não chamar `next()`, a requisição não avança para o próximo middleware.

---

## 2. Exemplo Básico

```js
const express = require('express');
const app = express();

// Middleware de logging
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});

app.get('/', (req, res) => {
  res.send('Hello, world!');
});

app.listen(3000);
```

---

## 3. Tipos de Middlewares

- **Globais**: Aplicados a todas as rotas (`app.use`).
- **De rota**: Aplicados a rotas específicas.
- **De tratamento de erros**: Possuem 4 parâmetros (err, req, res, next).

---

## 4. Ordem de Execução

- A ordem dos middlewares importa! Eles são executados na ordem em que são definidos no código.

---

## 5. Exemplos de Uso

- Autenticação e autorização
- Parsing de JSON (`express.json()`)
- Manipulação de cookies
- Logging de requisições
- Tratamento de erros

---

## 6. Referências

- [Documentação Express.js - Middleware](https://expressjs.com/pt-br/guide/using-middleware.html)
- [MDN Web Docs - Express middleware](https://developer.mozilla.org/pt-BR/docs/Learn/Server-side/Express_Nodejs/middleware)
