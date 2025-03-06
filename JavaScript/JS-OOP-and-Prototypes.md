# JavaScript: OOP e Prototypes
JavaScript usa um modelo de **protótipos** para implementar Programação Orientada a Objetos (OOP), diferente de linguagens baseadas em classes tradicionais como Java. Vamos explorar como isso funciona e as principais formas de criar objetos.

## Prototypes
Todo objeto em JS tem um **protótipo**, que é como ele herda propriedades e métodos. O protótipo é um objeto "pai" acessado via __proto__ ou `Object.getPrototypeOf()`. Quando você chama um método, JS primeiro procura no objeto; se não achar, sobe na cadeia de protótipos.
Exemplo:
```javascript
const animal = { fala: () => console.log("Som!") };
const cachorro = { latir: () => console.log("Au au!") };
Object.setPrototypeOf(cachorro, animal);
cachorro.latir(); // "Au au!"
cachorro.fala(); // "Som!" (herdado de animal)
```
`Object.setPrototypeOf`: Define o protótipo de um objeto manualmente.

A cadeia de protótipos termina em `Object.prototype`.

## Factory Functions
São funções que criam e retornam objetos, como uma "fábrica". Simples e flexíveis, mas não usam protótipos diretamente.
Exemplo:
```javascript
function criarPessoa(nome, idade) {
  return {
    nome,
    idade,
    dizerOi: () => console.log(`Oi, sou ${nome}!`)
  };
}
const p1 = criarPessoa("Igor", 25);
p1.dizerOi(); // "Oi, sou Igor!"
```
Vantagem: Fácil de entender e configurar.

Desvantagem: Cada objeto criado tem sua própria cópia dos métodos (mais memória).

## Constructor Functions
Funções construtoras usam `this` e são chamadas com `new`. Elas vinculam métodos ao protótipo automaticamente, economizando memória.
Exemplo:
```javascript
function Pessoa(nome, idade) {
  this.nome = nome;
  this.idade = idade;
}
Pessoa.prototype.dizerOi = function() {
  console.log(`Oi, sou ${this.nome}!`);
};
const p2 = new Pessoa("Igor", 25);
p2.dizerOi(); // "Oi, sou Igor!"
```
`new`: Cria um objeto, define seu protótipo e retorna o objeto.

Métodos no `prototype` são compartilhados entre instâncias.

## JavaScript Classes
Classes em JS (introduzidas no ES6) são uma sintaxe mais limpa para Constructor Functions. Por trás, ainda usam protótipos.
Exemplo:
```javascript
class Pessoa {
  constructor(nome, idade) {
    this.nome = nome;
    this.idade = idade;
  }
  dizerOi() {
    console.log(`Oi, sou ${this.nome}!`);
  }
}
const p3 = new Pessoa("Igor", 25);
p3.dizerOi(); // "Oi, sou Igor!"
```
`constructor`: Define as propriedades iniciais.

Métodos vão automaticamente para o `prototype`.

## Extends e Super
`extends` cria herança entre classes, e `super` chama o construtor ou métodos da classe "pai".
Exemplo:
```javascript
class Animal {
  constructor(tipo) {
    this.tipo = tipo;
  }
  fazerSom() {
    console.log("Algum som!");
  }
}
class Cachorro extends Animal {
  constructor(nome) {
    super("Cachorro"); // Chama o constructor de Animal
    this.nome = nome;
  }
  latir() {
    console.log("Au au!");
  }
}
const dog = new Cachorro("Rex");
console.log(dog.tipo); // "Cachorro"
dog.fazerSom(); // "Algum som!" (herdado)
dog.latir(); // "Au au!"
```
`extends`: Define a herança.

`super`: Acessa a classe pai (obrigatório no `constructor` de subclasses).

## Métodos Estáticos
Métodos estáticos pertencem à classe, não às instâncias. São chamados diretamente no nome da classe, sem precisar de `new`. Úteis para funções utilitárias relacionadas à classe.
Exemplo:
```javascript
class Calculadora {
  static somar(a, b) {
    return a + b;
  }
}
console.log(Calculadora.somar(5, 3)); // 8
// Não precisa criar uma instância!
```
`static`: Define o método como estático.

**Uso comum**: Ferramentas ou helpers (ex.: `Math.random()`).



## Resumo
**Prototypes**: Base da herança em JS.

**Factory Functions**: Simples, mas sem economia de memória.

**Constructor Functions**: Eficientes com protótipos.

**Classes**: Sintaxe moderna para OOP.

**extends/super**: Herança entre classes.

