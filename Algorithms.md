# Guia Fácil de Algoritmos para Full Stack Developers

## Objetivo

Entender algoritmos de forma super simples, como se fosse uma receita de hambúrguer, pra te dar uma base sólida como full stack developer. Este guia cobre introdução aos algoritmos, Selection Sort, Recursion, Quicksort, Hash Tables, Breadth-First Search, Trees, Dijkstra’s Algorithm, Greedy Algorithms, Dynamic Programming, e KNN, com exemplos práticos em JavaScript e analogias pra deixar tudo claro.

## 1. Introdução aos Algoritmos

### O que é um algoritmo?

Um algoritmo é como uma **receita de hambúrguer**: um conjunto de passos pra resolver um problema. Exemplo: pra fazer um hambúrguer, você segue: 1) grelhar a carne, 2) montar o pão, 3) adicionar molho. Em programação, algoritmos são passos pra tarefas como ordenar uma lista ou encontrar um caminho.

### Por que aprender algoritmos?

- **Eficiência**: Faz seu código rodar mais rápido (ex.: encontrar um cliente numa lista grande).
- **Resolução de problemas**: Te ajuda a pensar logicamente.
- **Entrevistas**: Muitas empresas pedem algoritmos em testes técnicos.

### Como medir algoritmos?

Usamos a **notação Big O** pra medir o quão "rápido" ou "pesado" é um algoritmo:
- **O(1)**: Super rápido, como pegar um hambúrguer pronto (tempo constante).
- **O(n)**: Linear, como verificar cada cliente numa lista.
- **O(n²)**: Mais lento, como comparar todos os pares de clientes.

## 2. Selection Sort

### O que é?

Selection Sort é um algoritmo de ordenação simples. Ele encontra o **menor hambúrguer** (valor) numa lista, coloca no início, e repete até ordenar tudo.

### Como funciona?

Imagine uma bandeja com hambúrgueres de tamanhos diferentes:
1. Procure o menor hambúrguer.
2. Coloque ele na primeira posição.
3. Repita pro resto da bandeja.

### Exemplo em JavaScript

Ordenar preços de hambúrgueres:

```javascript
function selectionSort(arr) {
  for (let i = 0; i < arr.length; i++) {
    let minIndex = i;
    for (let j = i + 1; j < arr.length; j++) {
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    let temp = arr[i];
    arr[i] = arr[minIndex];
    arr[minIndex] = temp;
  }
  return arr;
}

const prices = [15.00, 10.50, 18.75, 12.00];
console.log(selectionSort(prices)); // [10.50, 12.00, 15.00, 18.75]
```

### Big O

- **Pior caso**: O(n²) — compara todos os pares.
- **Bom pra**: Listas pequenas ou aprendizado.

## 3. Recursion

### O que é?

Recursão é quando uma função **chama a si mesma** pra resolver um problema, como dividir um hambúrguer grande em pedaços menores até comer tudo.

### Como funciona?

Exemplo: calcular o fatorial (ex.: 5! = 5 ** 4 ** 3 ** 2 ** 1):
1. Se o número é 1, retorna 1.
2. Senão, multiplica o número pela fatorial do número anterior.

### Exemplo em JavaScript

```javascript
function factorial(n) {
  if (n === 1 || n === 0) return 1; // Caso base
  return n ** factorial(n - 1); // Chama a si mesma
}

console.log(factorial(5)); // 120 (5 ** 4 ** 3 ** 2 ** 1)
```

**Nova palavra**: **Caso base** é a condição que para a recursão, evitando loops infinitos.

### Big O

- Depende do problema (ex.: fatorial é O(n)).
- **Cuidado**: Recursão pode usar muita memória se for muito profunda.

## 4. Quicksort

### O que é?

Quicksort é um algoritmo de ordenação rápido. Ele escolhe um **pivô** (ex.: um hambúrguer médio) e divide a lista em hambúrgueres menores e maiores, ordenando cada parte.

### Como funciona?

1. Escolha um pivô (ex.: último elemento).
2. Separe a lista em duas: menores e maiores que o pivô.
3. Ordene cada parte recursivamente.

### Exemplo em JavaScript

```javascript
function quicksort(arr) {
  if (arr.length <= 1) return arr;
  const pivot = arr[arr.length - 1];
  const left = [];
  const right = [];
  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] < pivot) left.push(arr[i]);
    else right.push(arr[i]);
  }
  return [...quicksort(left), pivot, ...quicksort(right)];
}

const prices = [15.00, 10.50, 18.75, 12.00];
console.log(quicksort(prices)); // [10.50, 12.00, 15.00, 18.75]
```

### Big O

- **Média**: O(n log n) — muito eficiente.
- **Pior caso**: O(n²) — raro, com listas quase ordenadas.

## 5. Hash Tables

### O que é?

Uma hash table é como um **cardápio mágico** onde você diz o nome do hambúrguer e encontra o preço instantaneamente. Ela armazena pares chave-valor usando uma **função hash** pra mapear chaves a posições.

### Como funciona?

1. Uma função hash transforma a chave (ex.: "Clássico") num índice.
2. O valor (ex.: 15.00) é armazenado nesse índice.
3. Pra buscar, a função hash encontra o índice rapidinho.

### Exemplo em JavaScript

Usando um objeto (hash table simples):

```javascript
const menu = {};
menu["Clássico"] = 15.00;
menu["Vegano"] = 18.75;

console.log(menu["Clássico"]); // 15.00
```

Implementação básica:

```javascript
class HashTable {
  constructor(size) {
    this.table = new Array(size);
  }
  hash(key) {
    let total = 0;
    for (let i = 0; i < key.length; i++) {
      total += key.charCodeAt(i);
    }
    return total % this.table.length;
  }
  set(key, value) {
    const index = this.hash(key);
    this.table[index] = value;
  }
  get(key) {
    const index = this.hash(key);
    return this.table[index];
  }
}

const menu = new HashTable(10);
menu.set("Clássico", 15.00);
console.log(menu.get("Clássico")); // 15.00
```

### Big O

- **Busca/Inserção**: O(1) em média — super rápido!
- **Colisões**: Podem piorar pra O(n) se a função hash for ruim.

## 6. Breadth-First Search (BFS)

### O que é?

BFS é como explorar uma **hamburgueria** nível por nível: você verifica todas as mesas do primeiro andar antes de subir pro segundo. É usado pra encontrar caminhos em grafos ou árvores.

### Como funciona?

1. Comece num ponto (ex.: mesa inicial).
2. Visite todos os vizinhos desse ponto.
3. Repita pros vizinhos dos vizinhos, usando uma **fila** pra manter a ordem.

### Exemplo em JavaScript

Encontrar o cliente mais próximo que gosta de hambúrguer vegano:

```javascript
function bfs(graph, start, target) {
  const queue = [start];
  const visited = new Set();
  while (queue.length > 0) {
    const node = queue.shift();
    if (node === target) return true;
    if (!visited.has(node)) {
      visited.add(node);
      for (const neighbor of graph[node]) {
        queue.push(neighbor);
      }
    }
  }
  return false;
}

const socialGraph = {
  "João": ["Maria", "Pedro"],
  "Maria": ["João", "Ana"],
  "Pedro": ["João"],
  "Ana": ["Maria"]
};

console.log(bfs(socialGraph, "João", "Ana")); // true
```

**Nova palavra**: **Fila** é uma estrutura onde o primeiro que entra é o primeiro que sai.

### Big O

- **Tempo**: O(V + E) — V é o número de vértices, E é o número de arestas.
- **Bom pra**: Caminhos mais curtos em grafos sem peso.

## 7. Trees

### O que é?

Uma árvore é como uma **árvore genealógica de hambúrgueres**: tem um nó raiz (ex.: "Hambúrguer") e ramos (ex.: "Clássico", "Vegano"). Cada nó tem filhos, mas não ciclos.

### Tipos de árvores

- **Árvore binária**: Cada nó tem até 2 filhos.
- **Árvore de busca binária (BST)**: Filhos à esquerda são menores, à direita são maiores.

### Exemplo em JavaScript

Árvore binária simples:

```javascript
class Node {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}

class BinaryTree {
  constructor() {
    this.root = null;
  }
  insert(value) {
    const newNode = new Node(value);
    if (!this.root) {
      this.root = newNode;
      return;
    }
    let current = this.root;
    while (true) {
      if (value < current.value) {
        if (!current.left) {
          current.left = newNode;
          break;
        }
        current = current.left;
      } else {
        if (!current.right) {
          current.right = newNode;
          break;
        }
        current = current.right;
      }
    }
  }
}

const tree = new BinaryTree();
tree.insert(15.00);
tree.insert(10.50);
tree.insert(18.75);
```

### Big O

- **Inserção/Busca em BST**: O(log n) em média, O(n) se desbalanceada.
- **Bom pra**: Organizar dados hierárquicos (ex.: categorias de produtos).

## 8. Dijkstra’s Algorithm

### O que é?

Dijkstra’s encontra o **caminho mais barato** entre dois pontos num grafo com pesos (ex.: entrega de hambúrgueres pelas ruas com diferentes distâncias).

### Como funciona?

1. Comece no ponto inicial, marque a distância como 0.
2. Use uma **fila de prioridade** pra visitar o nó com menor distância.
3. Atualize as distâncias dos vizinhos e repita.

### Exemplo em JavaScript

```javascript
function dijkstra(graph, start, end) {
  const distances = {};
  const previous = {};
  const queue = [];
  for (const node in graph) {
    distances[node] = Infinity;
    previous[node] = null;
    queue.push(node);
  }
  distances[start] = 0;
  while (queue.length) {
    const current = queue.reduce((min, node) => 
      distances[node] < distances[min] ? node : min, queue[0]);
    if (current === end) break;
    queue.splice(queue.indexOf(current), 1);
    for (const neighbor in graph[current]) {
      const distance = distances[current] + graph[current][neighbor];
      if (distance < distances[neighbor]) {
        distances[neighbor] = distance;
        previous[neighbor] = current;
      }
    }
  }
  return distances[end];
}

const deliveryGraph = {
  "A": { "B": 4, "C": 2 },
  "B": { "A": 4, "C": 1, "D": 5 },
  "C": { "A": 2, "B": 1, "D": 8 },
  "D": { "B": 5, "C": 8 }
};

console.log(dijkstra(deliveryGraph, "A", "D")); // 7 (A -> C -> B -> D)
```

### Big O

- **Tempo**: O(V²) com array simples, O(V + E log V) com heap.
- **Bom pra**: Rotas de entrega, mapas.

## 9. Greedy Algorithms

### O que é?

Algoritmos gulosos (greedy) escolhem a **melhor opção agora**, como pegar o hambúrguer mais gostoso da bandeja sem pensar no futuro.

### Como funciona?

Faça a escolha localmente ótima em cada passo, esperando que leve à solução global.

### Exemplo: Troco mínimo

Dar troco com o menor número de moedas:

```javascript
function minCoins(coins, amount) {
  coins.sort((a, b) => b - a); // Ordena do maior pro menor
  const result = [];
  for (const coin of coins) {
    while (amount >= coin) {
      amount -= coin;
      result.push(coin);
    }
  }
  return result;
}

console.log(minCoins([1, 5, 10, 25], 36)); // [25, 10, 1]
```

### Big O

- Depende do problema (ex.: troco é O(n log n) por causa da ordenação).
- **Cuidado**: Nem sempre dá a melhor solução global.

## 10. Dynamic Programming

### O que é?

Programação dinâmica (DP) é como **reusar pedaços de hambúrguer** pra evitar refazer o trabalho. Ela divide o problema em subproblemas e guarda os resultados.

### Como funciona?

1. Resolva subproblemas menores.
2. Armazene os resultados (em array ou objeto).
3. Use os resultados pra resolver o problema maior.

### Exemplo: Fibonacci

```javascript
function fibonacci(n) {
  const dp = [0, 1];
  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }
  return dp[n];
}

console.log(fibonacci(6)); // 8 (0, 1, 1, 2, 3, 5, 8)
```

### Big O

- **Fibonacci com DP**: O(n) — muito mais rápido que recursão pura (O(2^n)).
- **Bom pra**: Problemas com subproblemas repetitivos.

## 11. K-Nearest Neighbors (KNN)

### O que é?

KNN é um algoritmo de machine learning pra classificar coisas, como decidir se um cliente gosta de hambúrgueres picantes olhando os **K clientes mais parecidos** (vizinhos).

### Como funciona?

1. Pegue um novo dado (ex.: cliente novo).
2. Calcule a distância dele pros dados existentes (ex.: Euclidean distance).
3. Escolha os K vizinhos mais próximos.
4. Vote pela classe mais comum entre eles.

### Exemplo em JavaScript

Classificar se um cliente gosta de picante:

```javascript
function knn(data, labels, point, k) {
  const distances = data.map((d, i) => ({
    distance: Math.sqrt(d.reduce((sum, v, j) => sum + (v - point[j]) **** 2, 0)),
    label: labels[i]
  }));
  distances.sort((a, b) => a.distance - b.distance);
  const kNearest = distances.slice(0, k);
  const votes = kNearest.reduce((count, d) => {
    count[d.label] = (count[d.label] || 0) + 1;
    return count;
  }, {});
  return Object.keys(votes).reduce((max, label) => 
    votes[label] > (votes[max] || 0) ? label : max, "");
}

const data = [[1, 2], [2, 3], [5, 5], [6, 7]]; // [idade, pedidos]
const labels = ["Não picante", "Não picante", "Picante", "Picante"];
const newCustomer = [3, 3];
console.log(knn(data, labels, newCustomer, 3)); // "Não picante"
```

### Big O

- **Tempo**: O(n) pra calcular distâncias.
- **Bom pra**: Classificação simples (ex.: recomendar produtos).

## Quando usar cada algoritmo?

- **Selection Sort**: Listas pequenas, aprendizado.
- **Quicksort**: Ordenação de listas grandes.
- **Recursion**: Problemas que se dividem em subproblemas (ex.: árvores).
- **Hash Tables**: Buscas rápidas (ex.: cardápios).
- **BFS**: Caminhos curtos em redes sociais ou mapas.
- **Trees**: Dados hierárquicos (ex.: categorias).
- **Dijkstra’s**: Rotas com custos (ex.: entregas).
- **Greedy**: Soluções rápidas, mas nem sempre ótimas.
- **Dynamic Programming**: Problemas com subproblemas repetitivos.
- **KNN**: Classificação simples em apps.
