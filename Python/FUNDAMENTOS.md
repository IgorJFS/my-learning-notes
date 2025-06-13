# 🧱 01 - Fundamentos Essenciais do Python

Aqui está o DNA do Python. Dominando isso, você já consegue fazer 80% do trabalho. O resto são detalhes e bibliotecas.

## Variáveis e Tipos de Dados

Em Python, você não precisa declarar o tipo. O interpretador é malandro e descobre sozinho.

```python
# Texto (string)
nome = "Anakin Skywalker"
planeta = 'Tatooine'

# Números
idade = 22 # Inteiro (int)
altura = 1.88 # Ponto flutuante (float)

# Booleano (Verdadeiro/Falso)
tem_a_forca = True

# --- Estruturas de Dados ---

# Lista (mutável, ordenada, pode ter duplicados)
habilidades = ["Pilotar Pods", "Mecânica", "Sentir a Força"]
print(habilidades[0]) # Saída: Pilotar Pods
habilidades.append("Chorar por causa de areia")

# Tupla (imutável, ordenada, parece uma lista de gesso)
familia = ("Shmi Skywalker",) # A vírgula é crucial para tuplas de 1 item!

# Dicionário (mutável, chave-valor, super útil)
droide = {
    "nome": "C-3PO",
    "funcao": "Protocolo e Etiqueta",
    "criador": "Anakin Skywalker"
}
print(droide["funcao"]) # Saída: Protocolo e Etiqueta

# Set (mutável, não ordenado, não permite duplicados)
planetas_visitados = {"Tatooine", "Naboo", "Coruscant", "Tatooine"}
print(planetas_visitados) # Saída: {'Naboo', 'Tatooine', 'Coruscant'}
```
## Controle de Fluxo (Tomando Decisões)

# if, elif, else
```python
lado_da_forca = "sombrio"

if lado_da_forca == "luz":
  print("Que a Força esteja com você.")
elif lado_da_forca == "sombrio":
  print("Junte-se a mim e governaremos a galáxia!")
else:
  print("Você é só um fazendeiro de umidade, por enquanto.")

# For loops (para percorrer listas, dicionários, etc)
for habilidade in habilidades:
  print(f"Anakin sabe: {habilidade}")

# While loops (continua enquanto a condição for verdadeira)
poder = 10
while poder < 100:
  print(f"Poder atual: {poder}. Treinando mais...")
  poder += 10
print("Poder máximo atingido!")
```
## Funções (Para não repetir código)
```py
def saudar_jedi(nome, mestre="Yoda"):
  """Esta função saúda um Jedi. É um docstring, use para explicar!"""
  return f"Olá, {nome}. Seu mestre é o grande {mestre}."

# Chamando a função
saudacao = saudar_jedi("Obi-Wan")
print(saudacao) # Usa o valor padrão "Yoda"

saudacao_luke = saudar_jedi("Luke Skywalker", "Obi-Wan Kenobi")
print(saudacao_luke)
try:
  # Tenta fazer a divisão da Estrela da Morte por zero
  resultado = 1 / 0
except ZeroDivisionError:
  print("Almirante Ackbar, é uma armadilha! Não podemos dividir por zero!")
finally:
  # Isso sempre executa, com erro ou sem
  print("O Império tentou... e falhou.")
```