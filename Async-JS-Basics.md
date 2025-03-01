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
JavaScript é single-threaded (uma tarefa por vez), mas usa Web APIs (fornecidas pelo navegador) para lidar com assincronia, como setTimeout ou fetch.
```javascript

console.log("Start");
setTimeout(() => console.log("Timeout"), 1000);
console.log("End");
// Saída: "Start" -> "End" -> "Timeout" (após 1s)

O setTimeout é enviado à Web API, que o devolve ao call stack depois do tempo.
```
## Callback Hell
Callbacks aninhados levam a um código difícil de ler e manter, conhecido como "callback hell".
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
Promises são objetos que representam o eventual sucesso ou falha de uma operação assíncrona, com métodos .then() e .catch().
```javascript

const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Sucesso!"), 1000);
});
promise.then(result => console.log(result)) // "Sucesso!"
       .catch(error => console.log(error));
```
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
A palavra-chave async transforma uma função em assíncrona, retornando automaticamente uma Promise.
```javascript

async function fetchData() {
  return "Dados carregados";
}
fetchData().then(data => console.log(data)); // "Dados carregados"
```
## Await Keyword
O await pausa a execução de uma função async até que uma Promise seja resolvida, tornando o código mais legível.
```javascript

async function getData() {
  const result = await delay(1000);
  console.log(result); // "Esperou 1000ms"
}
getData();
```
## Handling Errors In Async Functions
Use try/catch para tratar erros em funções assíncronas com await.
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

