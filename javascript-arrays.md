# Arrays em JavaScript

## map
- **O que faz**: Transforma cada item de um array, retornando um novo array.
- **Exemplo**:
  ```javascript
  [1, 2, 3].map(x => x * 2) // [2, 4, 6]

## filter
- **O que faz**: Filtra itens de um array com base numa condição, retornando um novo array.
- **Exemplo**:
  ```javascript
  [1, 2, 3].filter(x => x > 1) // [2, 3]

## every
- **O que faz**: Testa se todos os itens do array passam numa condição, retornando `true` ou `false`.
- **Exemplo**:
  ```javascript
  [1, 2, 3].every(x => x > 0) // true
  [1, -2, 3].every(x => x > 0) // false

## some
- **O que faz**: Testa se pelo menos um item do array passa numa condição, retornando `true` ou `false`.
- **Exemplo**:
  ```javascript
  [1, -2, 3].some(x => x < 0) // true
  [1, 2, 3].some(x => x < 0) // false

## reduce
- **O que faz**: Reduz um array a um único valor, aplicando uma função acumuladora a cada item.
- **Exemplo**:
```javascript
  [1, 2, 3].reduce((acumulador, atual) => acumulador + atual) // 6
  [1, 2, 3].reduce((acumulador, atual) => acumulador * atual) // 6
```
## find
- **O que faz**: Retorna o primeiro elemento do array que satisfaz a condição especificada.
- **Exemplo**:
```javascript
  [1, 2, 3, 4].find(x => x > 2) // 3
  [10, 20, 30].find(x => x === 20) // 20
//segundo exemplo
  const usuarios = [
  { id: 1, nome: 'Ana' },
  { id: 2, nome: 'Bruno' },
  { id: 3, nome: 'Clara' }
];
const usuario = usuarios.find(user => user.id === 2);
console.log(usuario); // { id: 2, nome: 'Bruno' }
```
## forEach  
Executa uma função para cada elemento do array. Não retorna um novo array, apenas percorre os itens.

```javascript  
const numeros = [1, 2, 3];  
numeros.forEach(num => console.log(num * 2)); // 2, 4, 6  
```  
- **O que faz**: Executa a função para cada elemento. Aqui, dobra cada número e exibe.  
- **Quando usar**: Quando você precisa processar cada item sem criar um novo array (ex.: exibir dados, atualizar o DOM).

## includes  
Verifica se um valor está no array, retornando `true` ou `false`.

```javascript  
const frutas = ["maçã", "banana"];  
console.log(frutas.includes("maçã")); // true  
console.log(frutas.includes("laranja")); // false  
```  
- **O que faz**: Procura por um valor exato no array.  
- **Quando usar**: Para checar rapidamente se algo existe (ex.: validar entrada do usuário).

## indexOf  
Retorna o primeiro índice onde um valor é encontrado, ou `-1` se não estiver presente.

```javascript  
const cores = ["vermelho", "azul", "vermelho"];  
console.log(cores.indexOf("azul")); // 1  
console.log(cores.indexOf("verde")); // -1  
```  
- **O que faz**: Encontra a posição de um elemento no array.  
- **Quando usar**: Para localizar itens ou verificar duplicatas.

## push e pop  
`push` adiciona elementos ao final do array; `pop` remove o último elemento.

```javascript  
const pilha = [];  
pilha.push(1); // [1]  
pilha.push(2); // [1, 2]  
console.log(pilha.pop()); // 2, array agora [1]  
```  
- **O que faz**: `push` aumenta o array; `pop` diminui tirando do final.  
- **Quando usar**: Para gerenciar uma estrutura tipo pilha (ex.: função de desfazer).

## shift e unshift  
`shift` remove o primeiro elemento; `unshift` adiciona elementos no início.

```javascript  
const fila = [1, 2];  
console.log(fila.shift()); // 1, array agora [2]  
fila.unshift(0); // [0, 2]  
```  
- **O que faz**: `shift` tira do começo; `unshift` adiciona no começo.  
- **Quando usar**: Para comportamento tipo fila (ex.: processar itens em ordem).

## slice  
Extrai uma parte do array sem modificar o original, retornando um novo array.

```javascript  
const nums = [0, 1, 2, 3];  
console.log(nums.slice(1, 3)); // [1, 2]  
console.log(nums); // [0, 1, 2, 3] (inalterado)  
```  
- **O que faz**: Copia elementos do índice inicial até (mas não incluindo) o final.  
- **Quando usar**: Para pegar um pedaço do array (ex.: paginação).
 
## splice  
Adiciona ou remove elementos em uma posição específica, modificando o array original
```javascript  
const itens = [1, 2, 3];  
itens.splice(1, 1, "dois"); // Remove 1 item no índice 1, adiciona "dois"  
console.log(itens); // [1, "dois", 3]  
```  
- **O que faz**: Altera o array removendo e/ou inserindo elementos.  
- **Quando usar**: Para atualizações dinâmicas (ex.: editar uma lista).
