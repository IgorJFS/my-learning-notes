# O que é JavaScript?
JavaScript (JS) é uma linguagem de programação de alto nível, dinâmica e interpretada, criada em 1995 por Brendan Eich na Netscape. É um dos pilares da web moderna, junto com HTML e CSS, e hoje vai além do navegador com ambientes como Node.js.
## O que é?
**Linguagem de script**: Não precisa de compilação, roda direto no navegador ou em motores como V8 (Node.js).

**Dinâmica**: Tipos são resolvidos em tempo de execução (ex.: `let x = 5; x = "texto";` é válido).

**Multi-paradigma**: Suporta programação funcional, orientada a objetos (com protótipos) e imperativa.

## História Rápida
1995: Nasceu como "Mocha", virou "LiveScript", e depois "JavaScript" pra pegar carona na fama do Java.

1997: Padronizado como ECMAScript (ES). Hoje, usamos ES6+ (2015 em diante).

Hoje: É a linguagem mais usada no mundo (Stack Overflow, 2023), graças à web e ao Node.js.

## Por que é importante?
**Web interativa**: JS adiciona dinamismo (ex.: botões clicáveis, formulários) que HTML/CSS não fazem sozinhos.

**Full-stack**: Com Node.js, você usa JS pra front-end e back-end.

**Ecossistema**: Bibliotecas como React, Vue e Angular revolucionaram o desenvolvimento.

## Exemplo Básico
No navegador:
```html
<script>
  let nome = "Zackie";
  console.log(`Oi, ${nome}!`);
  alert("Bem-vindo ao JS!");
</script>
```
Mostra "Oi, Zackie!" no console e um alerta.

## Características
**Eventos**: Responde a ações do usuário (cliques, teclas).
```javascript
document.addEventListener("click", () => console.log("Clicou!"));
```

**Assíncrono**: Usa callbacks, Promises e async/await pra tarefas como requisições HTTP.

**Prototipagem**: Herança via protótipos, diferente de classes tradicionais.

## Dica
Comece com o básico (variáveis, funções) e evolua pra assincronia e frameworks!

