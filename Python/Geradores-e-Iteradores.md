# Geradores e Iteradores em Python

Geradores e iteradores são ferramentas poderosas em Python para trabalhar com grandes volumes de dados de forma eficiente, utilizando a abordagem "lazy" (sob demanda).

---

## 1. O que é um Iterador?

Um iterador é um objeto que implementa os métodos `__iter__()` e `__next__()`. Ele permite percorrer elementos de uma coleção, um por vez.

### Exemplo de Iterador:

```python
# Criando um iterador manualmente
class Contador:
    def __init__(self, inicio, fim):
        self.atual = inicio
        self.fim = fim

    def __iter__(self):
        return self

    def __next__(self):
        if self.atual > self.fim:
            raise StopIteration
        valor = self.atual
        self.atual += 1
        return valor

# Usando o iterador
contador = Contador(1, 5)
for numero in contador:
    print(numero)  # Saída: 1, 2, 3, 4, 5
```

---

## 2. O que é um Gerador?

Um gerador é uma função que utiliza a palavra-chave `yield` para produzir valores, um de cada vez, em vez de retornar todos de uma vez. Ele é mais simples de implementar do que um iterador manual.

### Exemplo de Gerador:

```python
def contador(inicio, fim):
    atual = inicio
    while atual <= fim:
        yield atual
        atual += 1

# Usando o gerador
for numero in contador(1, 5):
    print(numero)  # Saída: 1, 2, 3, 4, 5
```

---

## 3. Diferenças entre Iteradores e Geradores

| Característica         | Iterador                          | Gerador                          |
|------------------------|------------------------------------|-----------------------------------|
| Implementação          | Classe com `__iter__` e `__next__` | Função com `yield`               |
| Complexidade           | Mais verboso                      | Mais simples                     |
| Estado                 | Mantido manualmente               | Mantido automaticamente          |
| Uso de memória         | Pode ser maior                    | Mais eficiente (lazy evaluation) |

---

## 4. Geradores com Expressões

Geradores podem ser criados usando expressões semelhantes a listas, mas com parênteses.

```python
# Gerador com expressão
quadrados = (x**2 for x in range(1, 6))

for numero in quadrados:
    print(numero)  # Saída: 1, 4, 9, 16, 25
```

---

## 5. Aplicações Práticas

### 5.1 Processamento de Arquivos Grandes

```python
def ler_arquivo_em_pedacos(nome_arquivo, tamanho_pedaco):
    with open(nome_arquivo, "r") as arquivo:
        while True:
            pedaco = arquivo.read(tamanho_pedaco)
            if not pedaco:
                break
            yield pedaco

# Usando o gerador
for pedaco in ler_arquivo_em_pedacos("arquivo_grande.txt", 1024):
    print(pedaco)  # Processa o arquivo em pedaços de 1KB
```

### 5.2 Sequências Infinitas

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

# Usando o gerador
fib = fibonacci()
for _ in range(10):
    print(next(fib))  # Saída: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

---

## 6. Funções Built-in com Iteradores e Geradores

Python oferece várias funções que trabalham bem com iteradores e geradores:

- `map(func, iterável)`: Aplica uma função a cada elemento de um iterável.
- `filter(func, iterável)`: Filtra elementos de um iterável com base em uma função.
- `zip(*iteráveis)`: Combina elementos de iteráveis em tuplas.
- `itertools`: Módulo com ferramentas avançadas para iteradores.

### Exemplo com `itertools`:

```python
from itertools import islice

def numeros_infinito():
    n = 1
    while True:
        yield n
        n += 1

# Pegando apenas os 5 primeiros números de uma sequência infinita
for numero in islice(numeros_infinito(), 5):
    print(numero)  # Saída: 1, 2, 3, 4, 5
```

---

## 7. Melhores Práticas

1. **Use geradores para grandes volumes de dados**: Evite carregar tudo na memória de uma vez.
2. **Combine com funções built-in**: Use `map`, `filter` e `itertools` para manipular iteradores.
3. **Prefira geradores para sequências infinitas**: Como Fibonacci ou números primos.
4. **Documente bem**: Explique o que o gerador produz e como ele deve ser usado.

---

## 8. Referências

- [Python Documentation - Iterators](https://docs.python.org/3/library/stdtypes.html#typeiter)
- [Python Documentation - Generators](https://docs.python.org/3/howto/functional.html#generators)
- [Real Python - Generators](https://realpython.com/introduction-to-python-generators/)
