# 🐍 Python - Programação Orientada a Objetos (POO)

A Programação Orientada a Objetos em Python permite criar código mais organizado, reutilizável e fácil de manter. Aqui você aprenderá os conceitos fundamentais da POO.

---

## 1. Classes e Objetos

Uma classe é como um molde para criar objetos. Um objeto é uma instância de uma classe.

```python
class Cachorro:
    # Atributo de classe (compartilhado por todos os objetos)
    especie = "Canis lupus"
    
    # Método construtor
    def __init__(self, nome, raca, idade):
        self.nome = nome        # Atributo de instância
        self.raca = raca
        self.idade = idade
        self.energia = 100
    
    # Método de instância
    def latir(self):
        return f"{self.nome} está fazendo: Au au!"
    
    def brincar(self):
        if self.energia > 20:
            self.energia -= 20
            return f"{self.nome} brincou e gastou energia!"
        else:
            return f"{self.nome} está cansado demais para brincar."

# Criando objetos (instâncias)
rex = Cachorro("Rex", "Pastor Alemão", 3)
bella = Cachorro("Bella", "Golden Retriever", 2)

print(rex.latir())        # Rex está fazendo: Au au!
print(bella.brincar())    # Bella brincou e gastou energia!
print(f"Energia da Bella: {bella.energia}")  # 80
```

---

## 2. Herança

Permite criar classes filhas que herdam características da classe pai.

```python
class Animal:
    def __init__(self, nome, idade):
        self.nome = nome
        self.idade = idade
    
    def dormir(self):
        return f"{self.nome} está dormindo..."
    
    def comer(self):
        return f"{self.nome} está comendo."

class Gato(Animal):  # Gato herda de Animal
    def __init__(self, nome, idade, cor_pelo):
        super().__init__(nome, idade)  # Chama o construtor da classe pai
        self.cor_pelo = cor_pelo
    
    def miar(self):
        return f"{self.nome} faz: Miau!"
    
    def arranhar(self):
        return f"{self.nome} arranha o sofá!"

class Passaro(Animal):
    def __init__(self, nome, idade, envergadura):
        super().__init__(nome, idade)
        self.envergadura = envergadura
    
    def voar(self):
        return f"{self.nome} está voando!"

# Usando as classes
mimi = Gato("Mimi", 2, "cinza")
print(mimi.comer())     # Método herdado de Animal
print(mimi.miar())      # Método próprio do Gato

tweety = Passaro("Tweety", 1, 15)
print(tweety.dormir())  # Método herdado de Animal
print(tweety.voar())    # Método próprio do Passaro
```

---

## 3. Encapsulamento

Controla o acesso aos atributos e métodos da classe.

```python
class ContaBancaria:
    def __init__(self, titular, saldo_inicial=0):
        self.titular = titular
        self.__saldo = saldo_inicial  # Atributo privado (convenção)
        self._historico = []          # Atributo protegido (convenção)
    
    # Getter (para acessar atributo privado)
    @property
    def saldo(self):
        return self.__saldo
    
    # Setter (para modificar atributo privado com validação)
    @saldo.setter
    def saldo(self, valor):
        if valor >= 0:
            self.__saldo = valor
        else:
            raise ValueError("Saldo não pode ser negativo!")
    
    def depositar(self, valor):
        if valor > 0:
            self.__saldo += valor
            self._historico.append(f"Depósito: +R${valor}")
            return f"Depósito realizado! Saldo atual: R${self.__saldo}"
        else:
            return "Valor inválido para depósito!"
    
    def sacar(self, valor):
        if 0 < valor <= self.__saldo:
            self.__saldo -= valor
            self._historico.append(f"Saque: -R${valor}")
            return f"Saque realizado! Saldo atual: R${self.__saldo}"
        else:
            return "Saldo insuficiente ou valor inválido!"
    
    def extrato(self):
        return self._historico

# Usando a classe
conta = ContaBancaria("João", 1000)
print(conta.depositar(500))    # Depósito realizado! Saldo atual: R$1500
print(conta.sacar(200))        # Saque realizado! Saldo atual: R$1300
print(f"Saldo: R${conta.saldo}")  # Acessando via property

# Tentativa de acessar atributo privado diretamente (não recomendado)
# print(conta.__saldo)  # Isso causaria erro
```

---

## 4. Polimorfismo

Permite que objetos de diferentes classes respondam ao mesmo método de formas diferentes.

```python
class Veiculo:
    def acelerar(self):
        pass
    
    def frear(self):
        pass

class Carro(Veiculo):
    def acelerar(self):
        return "Carro acelerando com motor a combustão!"
    
    def frear(self):
        return "Carro freando com pastilhas de freio!"

class Bicicleta(Veiculo):
    def acelerar(self):
        return "Bicicleta acelerando com pedaladas!"
    
    def frear(self):
        return "Bicicleta freando com freios manuais!"

class Aviao(Veiculo):
    def acelerar(self):
        return "Avião acelerando com turbinas!"
    
    def frear(self):
        return "Avião freando com reverso das turbinas!"

# Polimorfismo em ação
veiculos = [Carro(), Bicicleta(), Aviao()]

for veiculo in veiculos:
    print(veiculo.acelerar())  # Cada um responde diferente
    print(veiculo.frear())
```

---

## 5. Métodos Especiais (Dunder Methods)

Métodos que começam e terminam com duplo underscore permitem personalizar o comportamento dos objetos.

```python
class Livro:
    def __init__(self, titulo, autor, paginas):
        self.titulo = titulo
        self.autor = autor
        self.paginas = paginas
    
    # Representação "oficial" do objeto
    def __repr__(self):
        return f"Livro('{self.titulo}', '{self.autor}', {self.paginas})"
    
    # Representação "amigável" do objeto
    def __str__(self):
        return f"'{self.titulo}' por {self.autor}"
    
    # Comparação de igualdade
    def __eq__(self, other):
        return (self.titulo == other.titulo and 
                self.autor == other.autor)
    
    # Comparação de tamanho (para ordenação)
    def __lt__(self, other):
        return self.paginas < other.paginas
    
    # Soma de livros (páginas)
    def __add__(self, other):
        return self.paginas + other.paginas

# Testando os métodos especiais
livro1 = Livro("1984", "George Orwell", 328)
livro2 = Livro("Dom Casmurro", "Machado de Assis", 200)

print(str(livro1))      # '1984' por George Orwell
print(repr(livro2))     # Livro('Dom Casmurro', 'Machado de Assis', 200)
print(livro1 > livro2)  # True (1984 tem mais páginas)
print(livro1 + livro2)  # 528 (soma das páginas)
```

---

## 6. Decoradores

Decoradores modificam o comportamento de funções ou classes.

```python
import time
from functools import wraps

# Decorador simples
def cronometrar(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        inicio = time.time()
        resultado = func(*args, **kwargs)
        fim = time.time()
        print(f"{func.__name__} executou em {fim - inicio:.4f} segundos")
        return resultado
    return wrapper

# Decorador com parâmetros
def repetir(vezes):
    def decorador(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(vezes):
                resultado = func(*args, **kwargs)
            return resultado
        return wrapper
    return decorador

# Usando os decoradores
@cronometrar
@repetir(3)
def calcular_fibonacci(n):
    if n <= 1:
        return n
    return calcular_fibonacci(n-1) + calcular_fibonacci(n-2)

resultado = calcular_fibonacci(10)
print(f"Resultado: {resultado}")
```

---

## 7. Context Managers

Gerenciam recursos automaticamente (arquivo, conexões, etc.).

```python
# Context manager personalizado
class GerenciadorArquivo:
    def __init__(self, nome_arquivo, modo):
        self.nome_arquivo = nome_arquivo
        self.modo = modo
        self.arquivo = None
    
    def __enter__(self):
        print(f"Abrindo arquivo {self.nome_arquivo}")
        self.arquivo = open(self.nome_arquivo, self.modo)
        return self.arquivo
    
    def __exit__(self, tipo_exc, valor_exc, traceback):
        print(f"Fechando arquivo {self.nome_arquivo}")
        if self.arquivo:
            self.arquivo.close()

# Usando o context manager
with GerenciadorArquivo("teste.txt", "w") as arquivo:
    arquivo.write("Olá, mundo!")
# Arquivo é fechado automaticamente
```

---

## 8. Referências

- [Python.org - Classes](https://docs.python.org/3/tutorial/classes.html)
- [Real Python - OOP](https://realpython.com/python3-object-oriented-programming/)
- [Python.org - Data Model](https://docs.python.org/3/reference/datamodel.html)
