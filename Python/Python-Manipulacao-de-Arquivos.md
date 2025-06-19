# Manipulação de Arquivos em Python

Trabalhar com arquivos é uma tarefa comum em Python. Abaixo estão as operações básicas de leitura, escrita e manipulação de modos e erros.

---

## 1. Abrindo Arquivos com `open()`

```python
# Sintaxe: open(caminho, modo, encoding=None)
# Modos:
# 'r'  → leitura (padrão)
# 'w'  → escrita (trunca ou cria)
# 'a'  → append (acrescenta)
# 'x'  → cria, erro se existir
# 'b'  → modo binário
# '+'  → leitura e escrita

arquivo = open('exemplo.txt', 'r', encoding='utf-8')
```

## 2. Leitura de Arquivos

```python
# Lê todo o conteúdo de uma vez
conteudo = arquivo.read()

# Lê linha a linha
linha = arquivo.readline()

# Lê todas as linhas em lista
linhas = arquivo.readlines()
```

## 3. Escrita em Arquivos

```python
# Modo 'w' cria ou trunca
with open('saida.txt', 'w', encoding='utf-8') as f:
    f.write('Olá, mundo!\n')
    f.writelines(['Linha 1\n', 'Linha 2\n'])

# Modo 'a' acrescenta ao final
with open('saida.txt', 'a', encoding='utf-8') as f:
    f.write('Mais conteúdo...')
```

## 4. Modo Binário

```python
with open('imagem.png', 'rb') as f:
    dados = f.read()

with open('copia.png', 'wb') as f:
    f.write(dados)
```

## 5. Fechando Arquivos

- Quando não usar `with`, feche com `arquivo.close()` para liberar recursos.

## 6. Tratamento de Erros e Exceções

```python
try:
    with open('nao_existe.txt', 'r') as f:
        dados = f.read()
except FileNotFoundError:
    print('Arquivo não encontrado.')
except IOError as e:
    print('Erro de I/O:', e)
```

---

## 7. Caminhos Relativos e Absolutos

```python
from os import path

# Caminho absoluto
abs_path = path.abspath('exemplo.txt')

# Verifica existência
if path.exists(abs_path):
    print('Existe!')
```

---

## 8. Exemplo Completo

```python
# Lê um arquivo, processa e salva resultado
import os

input_path = 'dados.csv'
output_path = 'resultado.txt'

if os.path.exists(input_path):
    with open(input_path, 'r', encoding='utf-8') as fin:
        linhas = fin.readlines()

    resultados = [linha.upper() for linha in linhas]

    with open(output_path, 'w', encoding='utf-8') as fout:
        fout.writelines(resultados)

    print('Processamento concluído:', output_path)
else:
    print('Arquivo de entrada não existe.')
```

---
