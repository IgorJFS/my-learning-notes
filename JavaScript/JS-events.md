# JavaScript Events

Este documento explica eventos comuns do JavaScript usados para interatividade no DOM, com foco no método `addEventListener`.

## O que é addEventListener?
O método `addEventListener` associa um evento a um elemento do DOM e executa uma função quando o evento ocorre. Ele é flexível e permite múltiplos ouvintes para o mesmo evento.

## Estrutura Básica
```javascript
element.addEventListener("event", function() {
  // Código a ser executado
});
```
## Exemplo com "click"
```javascript

const button = document.querySelector("button");
button.addEventListener("click", function() {
  alert("Botão clicado!");
});
```
## Formato Alternativo (com opções)
Você pode adicionar opções como once (executa só uma vez) ou capture (altera a fase de captura do evento):
```javascript

button.addEventListener("click", function() {
  alert("Clicou!");
}, { once: true }); // Executa apenas uma vez
```
## Eventos Comuns
Aqui está uma lista de eventos populares com uma breve explicação:
**click:** Dispara quando um elemento é clicado com o botão esquerdo do mouse.

- **drags:** Relacionado ao arrastar de um elemento (ex.: dragstart, drag, dragend).

- **drops**: Ocorre quando um elemento arrastado é solto (ex.: drop).

- **hovers**: Dispara quando o mouse passa sobre um elemento (ex.: mouseover, mouseout).

- **scrolls**: Ativado ao rolar a página ou um elemento (ex.: scroll).

- **form**: Eventos de formulário como change (mudança de valor) ou input (entrada de dados).

- **submission**: Dispara ao enviar um formulário (ex.: submit).

- **key presses**: Relacionado a teclas pressionadas (ex.: keydown, keypress, keyup).

- **focus/blur**: Quando um elemento ganha (focus) ou perde (blur) o foco.

- **mouse wheel**: Ativado ao rolar a roda do mouse (ex.: wheel).

- **double click**: Dispara com um duplo clique (ex.: dblclick).

- **copying**: Ocorre ao copiar conteúdo (ex.: copy).

- **pasting**: Dispara ao colar conteúdo (ex.: paste).

- **audio start**: Relacionado ao início de áudio (ex.: play).

- **screen resize**: Ativado ao redimensionar a janela (ex.: resize).

- **printing**: Dispara antes ou depois de imprimir (ex.: beforeprint, afterprint).

