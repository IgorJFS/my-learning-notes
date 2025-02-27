# Newer JavaScript Features

Este documento aborda algumas das funcionalidades mais modernas do JavaScript que facilitam a escrita de código mais limpo e expressivo.

## 1. Default Params
Permite definir valores padrão para parâmetros de funções, evitando verificações manuais de `undefined`.

```javascript
function greet(name = "Visitante") {
  return `Olá, ${name}!`;
}

console.log(greet());        // "Olá, Visitante!"
console.log(greet("João")); // "Olá, João!"
```
2. Spread in Function Calls
O operador spread (...) pode ser usado para "espalhar" os elementos de um array como argumentos de uma função.
```javascript

function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers)); // 6
```
3. Spread with Array Literals
Permite criar novos arrays combinando ou clonando outros arrays de forma concisa.
```javascript

const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];
console.log(arr2); // [1, 2, 3, 4]

// Clonando um array
const clone = [...arr1];
console.log(clone); // [1, 2]
```
4. Spread with Objects
Usado para copiar ou combinar propriedades de objetos de maneira simples.
```javascript

const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1, c: 3 };
console.log(obj2); // { a: 1, b: 2, c: 3 }

// Mesclando objetos
const defaults = { volume: 50, brightness: 80 };
const settings = { ...defaults, volume: 75 };
console.log(settings); // { volume: 75, brightness: 80 }
```
5. Rest Params
O operador rest (...) coleta todos os argumentos restantes em uma função em um único array.
```javascript

function logAll(first, ...rest) {
  console.log(first); // 1
  console.log(rest);  // [2, 3, 4, 5]
}

logAll(1, 2, 3, 4, 5);
```
6. Destructuring Arrays
Permite extrair valores de arrays diretamente em variáveis.
```javascript

const numbers = [10, 20, 30];
const [x, y, z] = numbers;
console.log(x, y, z); // 10 20 30

// Ignorando elementos
const [a, , c] = numbers;
console.log(a, c); // 10 30
```
7. Destructuring Objects
Extrai propriedades de objetos em variáveis, com a possibilidade de renomear.
```javascript

const person = { name: "Ana", age: 25 };
const { name, age } = person;
console.log(name, age); // "Ana" 25

// Renomeando
const { name: nome, age: idade } = person;
console.log(nome, idade); // "Ana" 25
```
8. Destructuring Params
Combina desestruturação com parâmetros de função para um código mais elegante.
```javascript

function display({ name, age }) {
  console.log(`Nome: ${name}, Idade: ${age}`);
}

const user = { name: "Carlos", age: 30 };
display(user); // "Nome: Carlos, Idade: 30"
