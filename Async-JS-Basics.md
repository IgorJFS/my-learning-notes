# Async JavaScript Basics

Este documento explora os conceitos fundamentais de programação assíncrona em JavaScript, essencial para lidar com operações que levam tempo, como requisições ou timers.

## The Call Stack
A pilha de chamadas (call stack) é onde o JavaScript executa funções em uma única thread. Ela funciona como uma pilha LIFO (Last In, First Out).

```javascript
function a() {
  console.log("a");
  b();
}
function b() {
  console.log("b");
}
a(); // Executa: "a" -> "b"

Cada função é empilhada e desempilhada conforme chamada e concluída.
```
## Web APIs & Single Threaded
JavaScript roda em uma única thread, mas delega tarefas assíncronas às Web APIs (fornecidas pelo navegador, como setTimeout, fetch, ou eventos DOM). Essas APIs trabalham fora da thread principal e devolvem resultados à call stack via uma fila de tarefas (task queue).
```javascript

console.log("Start");
setTimeout(() => console.log("Timeout"), 1000);
console.log("End");
// Saída: "Start" -> "End" -> "Timeout" (após 1s)
```
- **O que faz:** O setTimeout manda a função para a Web API, que espera 1 segundo e depois a coloca na fila para ser executada. Isso permite que o código continue sem esperar.

## Callback Hell
Quando usamos callbacks (funções passadas como argumentos para serem chamadas depois), muitos aninhamentos criam o "callback hell": um código confuso, difícil de ler e manter.
```javascript

setTimeout(() => {
  console.log("1");
  setTimeout(() => {
    console.log("2");
    setTimeout(() => {
      console.log("3");
    }, 1000);
  }, 1000);
}, 1000);

Problema: Código "piramidal" e propenso a erros.
```
## Promises
Uma Promise é um objeto que representa o resultado futuro de uma operação assíncrona. Ela pode estar em três estados: pendente (pending), resolvida (fulfilled) com sucesso, ou rejeitada (rejected) com erro. Usamos .then() para sucesso e .catch() para erros.
```javascript

const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Sucesso!"), 1000);
});
promise.then(result => console.log(result)) // "Sucesso!"
       .catch(error => console.log(error));
```
- **O que faz**: A Promise "promete" um valor futuro. Aqui, após 1s, ela resolve com "Sucesso!", que é capturado por .then(). Se der erro (reject), .catch() lida com isso. Isso evita o callback hell, pois podemos encadear ações.

## Creating Our Own Promises
Você pode criar suas próprias promises para encapsular lógica assíncrona.
```javascript

function delay(ms) {
  return new Promise(resolve => {
    setTimeout(() => resolve(`Esperou ${ms}ms`), ms);
  });
}
delay(2000).then(message => console.log(message)); // "Esperou 2000ms"
```
## Async Keyword
A palavra-chave async marca uma função como assíncrona, o que significa que ela sempre retorna uma Promise implicitamente. Isso simplifica o uso de código assíncrono e prepara para o uso de await.
```javascript

async function fetchData() {
  return "Dados carregados";
}
fetchData().then(data => console.log(data)); // "Dados carregados"
```
- **O que faz:** async garante que a função seja tratada como assíncrona, mesmo que não use await internamente. O retorno ("Dados carregados") é automaticamente embrulhado em uma Promise, que podemos acessar com .then().


## Await Keyword
O await só pode ser usado dentro de uma função async e pausa a execução até que uma Promise seja resolvida, tornando o código assíncrono mais parecido com síncrono e fácil de ler.
```javascript

async function getData() {
  const result = await delay(1000);
  console.log(result); // "Esperou 1000ms"
}
getData();
```
- **O que faz:** await espera a Promise de delay(1000) resolver antes de prosseguir. Isso evita encadeamentos de .then(), deixando o código mais linear e natural, como se fosse síncrono.

## Handling Errors In Async Functions
Em funções async, usamos try/catch para capturar erros de Promises rejeitadas, mantendo o tratamento de falhas simples e centralizado.
```javascript

async function fetchWithError() {
  try {
    const result = await new Promise((_, reject) => {
      setTimeout(() => reject("Erro!"), 1000);
    });
  } catch (error) {
    console.log("Capturou:", error); // "Capturou: Erro!"
  }
}
fetchWithError();
```
- **O que faz:** O try tenta executar o código assíncrono. Se a Promise for rejeitada (aqui, com "Erro!"), o catch captura a falha e a trata, evitando que o programa trave.


