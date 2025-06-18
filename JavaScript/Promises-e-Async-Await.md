# Promises e Async/Await em JavaScript

Lidar com operações assíncronas é essencial em JavaScript, especialmente em aplicações web. Promises e async/await são ferramentas modernas que tornam o código assíncrono mais legível e fácil de gerenciar.

---

## 1. O que é uma Promise?

Uma Promise é um objeto que representa a eventual conclusão (ou falha) de uma operação assíncrona e seu valor resultante.

### Estados de uma Promise:
- **Pending**: A operação ainda não foi concluída.
- **Fulfilled**: A operação foi concluída com sucesso.
- **Rejected**: A operação falhou.

```javascript
const promessa = new Promise((resolve, reject) => {
  const sucesso = true; // Simula sucesso ou falha

  if (sucesso) {
    resolve("Operação bem-sucedida!");
  } else {
    reject("Algo deu errado.");
  }
});

promessa
  .then((resultado) => console.log(resultado)) // Operação bem-sucedida!
  .catch((erro) => console.error(erro));       // Algo deu errado.
```

---

## 2. Encadeamento de Promises

Promises podem ser encadeadas para executar operações assíncronas em sequência.

```javascript
function buscarDados() {
  return new Promise((resolve) => {
    setTimeout(() => resolve("Dados carregados"), 2000);
  });
}

function processarDados(dados) {
  return new Promise((resolve) => {
    setTimeout(() => resolve(`${dados} processados`), 2000);
  });
}

buscarDados()
  .then((dados) => processarDados(dados))
  .then((resultado) => console.log(resultado)) // Dados carregados processados
  .catch((erro) => console.error(erro));
```

---

## 3. Async/Await

Async/await é uma sintaxe mais limpa e legível para trabalhar com Promises. Ele permite escrever código assíncrono como se fosse síncrono.

### Exemplo:

```javascript
async function executar() {
  try {
    const dados = await buscarDados();
    const resultado = await processarDados(dados);
    console.log(resultado); // Dados carregados processados
  } catch (erro) {
    console.error(erro);
  }
}

executar();
```

---

## 4. Promise.all e Promise.race

### Promise.all
Executa várias Promises em paralelo e retorna quando todas forem resolvidas ou uma for rejeitada.

```javascript
const p1 = Promise.resolve("Primeira");
const p2 = new Promise((resolve) => setTimeout(() => resolve("Segunda"), 2000));
const p3 = Promise.resolve("Terceira");

Promise.all([p1, p2, p3])
  .then((resultados) => console.log(resultados)) // ["Primeira", "Segunda", "Terceira"]
  .catch((erro) => console.error(erro));
```

### Promise.race
Retorna a primeira Promise que for resolvida ou rejeitada.

```javascript
Promise.race([p1, p2, p3])
  .then((resultado) => console.log(resultado)) // Primeira
  .catch((erro) => console.error(erro));
```

---

## 5. Tratamento de Erros

Sempre trate erros em operações assíncronas para evitar falhas inesperadas.

```javascript
async function executarComErro() {
  try {
    const resultado = await Promise.reject("Erro simulado");
    console.log(resultado);
  } catch (erro) {
    console.error("Erro capturado:", erro);
  }
}

executarComErro();
```

---

## 6. Melhores Práticas

1. **Use async/await para legibilidade**: Prefira async/await em vez de encadeamento excessivo de `.then()`.
2. **Sempre trate erros**: Use `.catch()` ou `try/catch` para capturar erros.
3. **Evite callback hell**: Substitua callbacks aninhados por Promises ou async/await.
4. **Combine Promises com Promise.all**: Para executar tarefas paralelas de forma eficiente.
5. **Evite bloquear o event loop**: Não use operações síncronas em código assíncrono.

---

## 7. Referências

- [MDN Web Docs - Promises](https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN Web Docs - Async/Await](https://developer.mozilla.org/pt-BR/docs/Learn/JavaScript/Asynchronous/Promises)
- [JavaScript.info - Promises](https://javascript.info/promise-basics)
