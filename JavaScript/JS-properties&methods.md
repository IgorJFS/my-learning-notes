# JavaScript Properties & Methods

Este documento explora propriedades e métodos comuns do JavaScript para manipulação do DOM (Document Object Model).

## classList
Propriedade que retorna ou manipula as classes de um elemento como um objeto `DOMTokenList`.

```javascript
const elem = document.querySelector("div");
elem.classList.add("active");       // Adiciona a classe "active"
elem.classList.remove("active");    // Remove a classe "active"
elem.classList.toggle("active");    // Alterna a classe "active"
console.log(elem.classList);        // Lista todas as classes
```
## getAttribute()
Método que retorna o valor de um atributo específico de um elemento.
```javascript

const link = document.querySelector("a");
const href = link.getAttribute("href");
console.log(href); // Ex.: "https://exemplo.com"
```
## setAttribute()
Método que define ou atualiza o valor de um atributo em um elemento.
```javascript

const img = document.querySelector("img");
img.setAttribute("src", "nova-imagem.jpg");
img.setAttribute("alt", "Nova imagem");
```
## appendChild()
Método que adiciona um nó como último filho de um elemento pai.
```javascript

const parent = document.querySelector("div");
const newChild = document.createElement("p");
parent.appendChild(newChild); // Adiciona <p> como último filho
```
## append()
Método que adiciona múltiplos nós ou strings como filhos no final de um elemento.
```javascript

const div = document.querySelector("div");
div.append("Texto direto", document.createElement("span"));
// Adiciona texto e um <span> no final
```
## prepend()
Método que adiciona múltiplos nós ou strings como filhos no início de um elemento.
```javascript

const div = document.querySelector("div");
div.prepend("Primeiro texto", document.createElement("b"));
// Adiciona texto e um <b> no início
```
## removeChild()
Método que remove um nó filho específico de um elemento pai.
```javascript

const parent = document.querySelector("div");
const child = parent.querySelector("p");
parent.removeChild(child); // Remove o <p> do div
```
## remove()
Método que remove o próprio elemento do DOM.
```javascript

const elem = document.querySelector("span");
elem.remove(); // Remove o <span> diretamente
```
## createElement()
Método que cria um novo elemento HTML.
```javascript

const newDiv = document.createElement("div");
newDiv.textContent = "Novo elemento";
document.body.appendChild(newDiv); // Adiciona ao body
```
## innerText
Propriedade que define ou retorna o texto visível dentro de um elemento, ignorando estilos ocultos.
```javascript

const p = document.querySelector("p");
p.innerText = "Novo texto";      // Define o texto
console.log(p.innerText);        // "Novo texto"
```
## textContent
Propriedade que define ou retorna todo o texto de um elemento, incluindo conteúdo oculto.
```javascript

const div = document.querySelector("div");
div.textContent = "Texto visível e oculto";
console.log(div.textContent); // Retorna todo o texto, mesmo de <script> ou <style>
```
## innerHTML
Propriedade que define ou retorna o conteúdo HTML dentro de um elemento.
```javascript

const div = document.querySelector("div");
div.innerHTML = "<p>Texto com <b>negrito</b></p>";
console.log(div.innerHTML); // "<p>Texto com <b>negrito</b></p>"
```
## value
Propriedade que define ou retorna o valor de elementos de formulário como <input> ou <textarea>.
```javascript

const input = document.querySelector("input");
input.value = "Novo valor";
console.log(input.value); // "Novo valor"
```
## parentElement
Propriedade que retorna o elemento pai de um nó.
```javascript

const span = document.querySelector("span");
const parent = span.parentElement;
console.log(parent); // Ex.: <div> que contém o <span>
```
## children
Propriedade que retorna uma coleção (HTMLCollection) dos elementos filhos de um nó.
```javascript

const div = document.querySelector("div");
const kids = div.children;
console.log(kids); // Lista de filhos como <p>, <span>, etc.
```
## nextSibling
Propriedade que retorna o próximo nó irmão (pode ser elemento ou texto).
```javascript

const elem = document.querySelector("p");
const next = elem.nextSibling;
console.log(next); // Próximo nó, pode ser texto ou elemento
```
## previousSibling
Propriedade que retorna o nó irmão anterior (pode ser elemento ou texto).
```javascript

const elem = document.querySelector("p");
const prev = elem.previousSibling;
console.log(prev); // Nó anterior, pode ser texto ou elemento
```
## style
Propriedade que acessa ou define estilos CSS inline de um elemento.
```javascript

const div = document.querySelector("div");
div.style.backgroundColor = "blue";
div.style.fontSize = "16px";
console.log(div.style); // Objeto com estilos inline

