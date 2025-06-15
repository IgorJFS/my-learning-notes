# Polimorfismo em Python

O polimorfismo é um dos quatro pilares da Programação Orientada a Objetos, permitindo que objetos de diferentes classes respondam ao mesmo método de formas diferentes, tornando o código mais flexível e reutilizável.

---

## 1. O que é Polimorfismo?

A palavra "polimorfismo" vem do grego e significa "muitas formas". Na programação, refere-se à capacidade de objetos de classes diferentes responderem de maneiras distintas a um mesmo método ou operação.

O polimorfismo permite escrever código que trabalha com uma superclasse, mas pode operar em qualquer uma de suas subclasses sem conhecer os detalhes específicos de cada uma.

---

## 2. Tipos de Polimorfismo em Python

### 2.1 Polimorfismo de Sobrecarga (Duck Typing)

Python não suporta sobrecarga de métodos como outras linguagens, mas usa "duck typing" ("se anda como um pato e grasna como um pato, provavelmente é um pato").

```python
# Duck typing em ação
def fazer_animal_falar(animal):
    # Se o objeto tem um método "falar", usamos
    # Não importa qual classe seja
    print(animal.falar())

class Cachorro:
    def falar(self):
        return "Au au!"

class Gato:
    def falar(self):
        return "Miau!"

class Pato:
    def falar(self):
        return "Quack!"

# Usando o polimorfismo
rex = Cachorro()
felix = Gato()
donald = Pato()

# A mesma função funciona com diferentes objetos
fazer_animal_falar(rex)    # Au au!
fazer_animal_falar(felix)  # Miau!
fazer_animal_falar(donald) # Quack!
```

### 2.2 Polimorfismo de Herança

Implementação de métodos com a mesma assinatura em classes diferentes dentro de uma hierarquia.

```python
class Animal:
    def __init__(self, nome):
        self.nome = nome
    
    def fazer_som(self):
        # Método base que será sobrescrito
        raise NotImplementedError("Subclasses devem implementar este método")
    
    def apresentar(self):
        return f"Olá, eu sou {self.nome} e faço: {self.fazer_som()}"

class Cachorro(Animal):
    def fazer_som(self):  # Sobrescrevendo o método base
        return "Au au!"

class Gato(Animal):
    def fazer_som(self):  # Sobrescrevendo o método base
        return "Miau!"

class Vaca(Animal):
    def fazer_som(self):  # Sobrescrevendo o método base
        return "Muuu!"

# Criando objetos
animais = [
    Cachorro("Rex"),
    Gato("Felix"),
    Vaca("Mimosa")
]

# Polimorfismo em ação - mesmo método, comportamentos diferentes
for animal in animais:
    print(animal.apresentar())

# Saída:
# Olá, eu sou Rex e faço: Au au!
# Olá, eu sou Felix e faço: Miau!
# Olá, eu sou Mimosa e faço: Muuu!
```

### 2.3 Polimorfismo com Funções Internas

Funções internas do Python também são polimórficas, funcionando com diferentes tipos:

```python
# A função len() é polimórfica
print(len("Python"))      # 6 (string)
print(len([1, 2, 3, 4]))  # 4 (lista)
print(len({"a": 1, "b": 2}))  # 2 (dicionário)

# O operador + é polimórfico
print(10 + 5)         # 15 (soma numérica)
print("Olá " + "Mundo")  # "Olá Mundo" (concatenação de strings)
print([1, 2] + [3, 4])   # [1, 2, 3, 4] (concatenação de listas)
```

---

## 3. Interfaces em Python

Python não tem interfaces formais como Java, mas usa Abstract Base Classes (ABCs) para definir contratos:

```python
from abc import ABC, abstractmethod

# Interface/Classe abstrata
class FormaGeometrica(ABC):
    @abstractmethod
    def area(self):
        pass
    
    @abstractmethod
    def perimetro(self):
        pass
    
    def descricao(self):
        return f"Área: {self.area()}, Perímetro: {self.perimetro()}"

class Retangulo(FormaGeometrica):
    def __init__(self, largura, altura):
        self.largura = largura
        self.altura = altura
    
    def area(self):
        return self.largura * self.altura
    
    def perimetro(self):
        return 2 * (self.largura + self.altura)

class Circulo(FormaGeometrica):
    def __init__(self, raio):
        self.raio = raio
    
    def area(self):
        import math
        return math.pi * self.raio ** 2
    
    def perimetro(self):
        import math
        return 2 * math.pi * self.raio

# Usando polimorfismo com classes que seguem a interface
def imprimir_info_forma(forma):
    print(f"Forma: {type(forma).__name__}")
    print(f"Área: {forma.area()}")
    print(f"Perímetro: {forma.perimetro()}")
    print("---")

# Criando objetos
formas = [
    Retangulo(5, 10),
    Circulo(7)
]

# O mesmo código funciona para diferentes formas
for forma in formas:
    imprimir_info_forma(forma)
```

---

## 4. Exemplo Prático: Sistema de Pagamentos

```python
from abc import ABC, abstractmethod
from datetime import datetime

class MetodoPagamento(ABC):
    @abstractmethod
    def processar_pagamento(self, valor):
        pass
    
    @abstractmethod
    def gerar_recibo(self, valor, id_transacao):
        pass

class CartaoCredito(MetodoPagamento):
    def __init__(self, numero, titular, validade):
        self.numero = numero
        self.titular = titular
        self.validade = validade
    
    def processar_pagamento(self, valor):
        # Simula processamento de pagamento
        print(f"Processando pagamento de R${valor:.2f} via cartão de crédito...")
        id_transacao = f"CC-{datetime.now().strftime('%Y%m%d%H%M%S')}"
        print(f"Pagamento aprovado! ID: {id_transacao}")
        return id_transacao
    
    def gerar_recibo(self, valor, id_transacao):
        return f"""
        RECIBO DE PAGAMENTO - CARTÃO DE CRÉDITO
        ----------------------------------------
        Valor: R${valor:.2f}
        Data: {datetime.now().strftime('%d/%m/%Y %H:%M:%S')}
        Titular: {self.titular}
        Cartão: **** **** **** {self.numero[-4:]}
        ID Transação: {id_transacao}
        """

class Pix(MetodoPagamento):
    def __init__(self, chave_pix):
        self.chave_pix = chave_pix
    
    def processar_pagamento(self, valor):
        # Simula processamento de pagamento
        print(f"Processando pagamento de R${valor:.2f} via PIX...")
        id_transacao = f"PIX-{datetime.now().strftime('%Y%m%d%H%M%S')}"
        print(f"Pagamento aprovado! ID: {id_transacao}")
        return id_transacao
    
    def gerar_recibo(self, valor, id_transacao):
        return f"""
        RECIBO DE PAGAMENTO - PIX
        ------------------------
        Valor: R${valor:.2f}
        Data: {datetime.now().strftime('%d/%m/%Y %H:%M:%S')}
        Chave PIX: {self.chave_pix[:5]}...
        ID Transação: {id_transacao}
        """

class Boleto(MetodoPagamento):
    def __init__(self, codigo_barras):
        self.codigo_barras = codigo_barras
    
    def processar_pagamento(self, valor):
        # Simula processamento de pagamento
        print(f"Processando pagamento de R${valor:.2f} via boleto...")
        id_transacao = f"BOL-{datetime.now().strftime('%Y%m%d%H%M%S')}"
        print(f"Pagamento aprovado! ID: {id_transacao}")
        return id_transacao
    
    def gerar_recibo(self, valor, id_transacao):
        return f"""
        RECIBO DE PAGAMENTO - BOLETO
        ---------------------------
        Valor: R${valor:.2f}
        Data: {datetime.now().strftime('%d/%m/%Y %H:%M:%S')}
        Código: {self.codigo_barras[:10]}...
        ID Transação: {id_transacao}
        """

class Caixa:
    def finalizar_compra(self, valor, metodo_pagamento):
        print(f"\nFinalizando compra no valor de R${valor:.2f}")
        
        # Polimorfismo: não importa qual método de pagamento, a interface é a mesma
        id_transacao = metodo_pagamento.processar_pagamento(valor)
        
        # Gera e imprime recibo
        recibo = metodo_pagamento.gerar_recibo(valor, id_transacao)
        print("\nGerando recibo...")
        print(recibo)
        
        print("Compra finalizada com sucesso!")

# Usando o sistema
caixa = Caixa()

# Cliente 1 - Cartão de crédito
cartao = CartaoCredito("1234567890123456", "João Silva", "12/2025")
caixa.finalizar_compra(152.50, cartao)

# Cliente 2 - PIX
pix = Pix("joao@email.com")
caixa.finalizar_compra(89.90, pix)

# Cliente 3 - Boleto
boleto = Boleto("34191790010104351004791020150008291070026000")
caixa.finalizar_compra(210.75, boleto)
```

---

## 5. Implementando Protocolos (Interfaces Estruturais)

A partir do Python 3.8, temos os Protocolos do módulo `typing` que formalizam o duck typing:

```python
from typing import Protocol, runtime_checkable

# Definindo o protocolo
@runtime_checkable
class Imprimivel(Protocol):
    def imprimir(self) -> str:
        ...

# Classes que implementam o protocolo implicitamente
class Documento:
    def __init__(self, conteudo):
        self.conteudo = conteudo
    
    def imprimir(self):
        return f"Documento: {self.conteudo}"

class Foto:
    def __init__(self, imagem):
        self.imagem = imagem
    
    def imprimir(self):
        return f"Imprimindo foto: {self.imagem}"

class Texto:
    def __init__(self, texto):
        self.texto = texto
    
    def imprimir(self):
        return f"Texto: {self.texto}"

# Verificando se objetos implementam o protocolo
def imprimir_item(item):
    if isinstance(item, Imprimivel):
        print(item.imprimir())
    else:
        print("Este item não pode ser impresso")

# Testando
doc = Documento("Relatório Anual")
foto = Foto("paisagem.jpg")
texto = Texto("Olá, mundo!")
numero = 42

imprimir_item(doc)    # Documento: Relatório Anual
imprimir_item(foto)   # Imprimindo foto: paisagem.jpg
imprimir_item(texto)  # Texto: Olá, mundo!
imprimir_item(numero) # Este item não pode ser impresso
```

---

## 6. Benefícios do Polimorfismo

1. **Código mais flexível**: Facilita trabalhar com objetos de diferentes classes.
2. **Extensibilidade**: Facilita a adição de novos tipos/comportamentos.
3. **Modularidade**: Encapsula comportamentos específicos dentro das próprias classes.
4. **Manutenção simplificada**: Alterações em uma classe não afetam o código cliente.
5. **Desacoplamento**: Reduz dependências diretas entre componentes do código.

---

## 7. Quando usar Polimorfismo

- Ao trabalhar com diferentes objetos que compartilham comportamentos similares.
- Quando você deseja tratar objetos de diferentes tipos de maneira uniforme.
- Para construir frameworks e bibliotecas extensíveis.
- Ao implementar padrões de design como Strategy, Command, Visitor.

---

