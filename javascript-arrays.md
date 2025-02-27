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