# Encapsulamento em Python

O encapsulamento é um dos pilares da Programação Orientada a Objetos que permite esconder detalhes de implementação e proteger dados dentro de uma classe.

---

## 1. O que é Encapsulamento?

O encapsulamento consiste em limitar o acesso direto aos atributos e métodos de um objeto, disponibilizando uma interface controlada para manipulá-los. Isso ajuda a:

- Proteger dados sensíveis
- Prevenir alterações indesejadas
- Manter a integridade do objeto
- Reduzir dependências externas

---

## 2. Níveis de Acesso em Python

Python não tem modificadores de acesso como `private` ou `protected` de outras linguagens, mas segue convenções:

```python
class Funcionario:
    def __init__(self, nome, salario):
        self.nome = nome           # Público (acessível de qualquer lugar)
        self._setor = "TI"         # Protegido (convenção: não acesse diretamente)
        self.__salario = salario   # Privado (name mangling: difícil acesso externo)
```

- **Público**: Atributos e métodos normais (sem underscores)
- **Protegido**: Prefixo com um underscore (`_atributo`) - convenção
- **Privado**: Prefixo com dois underscores (`__atributo`) - name mangling

---

## 3. Name Mangling

Python modifica internamente o nome de atributos com dois underscores:

```python
funcionario = Funcionario("Ana", 5000)
print(funcionario.nome)       # Ana (acesso direto)
print(funcionario._setor)     # TI (acesso direto, mas não recomendado)
# print(funcionario.__salario)  # Erro! AttributeError

# O acesso ainda é possível, mas com nome modificado:
print(funcionario._Funcionario__salario)  # 5000
```

O Python renomeia internamente `__salario` para `_Funcionario__salario`, tornando o acesso difícil, mas não impossível.

---

## 4. Getters e Setters (métodos de acesso)

Fornecem acesso controlado aos atributos:

```python
class Pessoa:
    def __init__(self, nome, idade):
        self.__nome = nome
        self.__idade = idade
    
    # Métodos getters
    def get_nome(self):
        return self.__nome
    
    def get_idade(self):
        return self.__idade
    
    # Métodos setters
    def set_nome(self, novo_nome):
        if isinstance(novo_nome, str) and novo_nome.strip():
            self.__nome = novo_nome
        else:
            raise ValueError("Nome inválido")
    
    def set_idade(self, nova_idade):
        if isinstance(nova_idade, int) and 0 <= nova_idade <= 150:
            self.__idade = nova_idade
        else:
            raise ValueError("Idade deve ser um inteiro entre 0 e 150")

# Usando getters e setters
p = Pessoa("Carlos", 30)
print(p.get_nome())  # Carlos

p.set_idade(31)
print(p.get_idade())  # 31

# p.set_idade(-5)  # Erro! ValueError: Idade deve ser um inteiro entre 0 e 150
```

---

## 5. Properties (abordagem Pythônica)

É o modo preferido de implementar encapsulamento em Python:

```python
class Produto:
    def __init__(self, nome, preco):
        self._nome = nome
        self._preco = preco
    
    # Property para nome
    @property
    def nome(self):
        return self._nome
    
    @nome.setter
    def nome(self, valor):
        if not valor:
            raise ValueError("Nome não pode ser vazio")
        self._nome = valor
    
    # Property para preço
    @property
    def preco(self):
        return self._preco
    
    @preco.setter
    def preco(self, valor):
        if valor < 0:
            raise ValueError("Preço não pode ser negativo")
        self._preco = valor
    
    # Property calculada (somente leitura)
    @property
    def preco_com_imposto(self):
        return self._preco * 1.1  # 10% de imposto

# Usando properties - sintaxe limpa!
notebook = Produto("Notebook", 3000)
print(notebook.nome)  # Notebook
notebook.preco = 3500  # Usa o setter
print(notebook.preco)  # 3500
print(notebook.preco_com_imposto)  # 3850.0

# notebook.preco = -100  # Erro! ValueError: Preço não pode ser negativo
# notebook.preco_com_imposto = 4000  # Erro! AttributeError: can't set attribute
```

---

## 6. Properties vs Getters/Setters Tradicionais

**Getters/Setters tradicionais**:
- Comum em Java e outras linguagens
- Requer chamadas de métodos explícitas: `objeto.get_atributo()`, `objeto.set_atributo(valor)`
- Mais verboso

**Properties**:
- Abordagem Pythônica
- Sintaxe limpa: `objeto.atributo`, `objeto.atributo = valor`
- Transparente para quem usa a classe
- Permite adicionar validações e lógica sem alterar a interface

---

## 7. Exemplo Completo: Conta Bancária

```python
class ContaBancaria:
    def __init__(self, titular, saldo_inicial=0):
        self.__titular = titular
        self.__saldo = saldo_inicial
        self.__ativo = True
        self.__operacoes = []
        
        # Registra operação inicial
        if saldo_inicial > 0:
            self.__registrar_operacao("abertura", saldo_inicial)
    
    @property
    def titular(self):
        return self.__titular
    
    @property
    def saldo(self):
        return self.__saldo
    
    @property
    def ativo(self):
        return self.__ativo
    
    @property
    def extrato(self):
        return self.__operacoes.copy()  # Retorna uma cópia para evitar modificação
    
    def __registrar_operacao(self, tipo, valor):
        import datetime
        data = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.__operacoes.append({
            "tipo": tipo,
            "valor": valor,
            "data": data,
            "saldo_resultante": self.__saldo
        })
    
    def depositar(self, valor):
        if not self.__ativo:
            return "Conta desativada. Operação não permitida."
            
        if valor <= 0:
            return "Valor de depósito deve ser positivo."
            
        self.__saldo += valor
        self.__registrar_operacao("depósito", valor)
        
        return f"Depósito de R${valor:.2f} realizado com sucesso. Saldo atual: R${self.__saldo:.2f}"
    
    def sacar(self, valor):
        if not self.__ativo:
            return "Conta desativada. Operação não permitida."
            
        if valor <= 0:
            return "Valor de saque deve ser positivo."
            
        if valor > self.__saldo:
            return f"Saldo insuficiente. Seu saldo atual é R${self.__saldo:.2f}"
            
        self.__saldo -= valor
        self.__registrar_operacao("saque", valor)
        
        return f"Saque de R${valor:.2f} realizado com sucesso. Saldo atual: R${self.__saldo:.2f}"
    
    def desativar(self):
        if self.__saldo != 0:
            return "Não é possível desativar conta com saldo não-zero."
            
        self.__ativo = False
        self.__registrar_operacao("desativação", 0)
        
        return "Conta desativada com sucesso."

# Uso da classe
conta = ContaBancaria("Maria Silva", 1000)

print(conta.titular)  # Maria Silva
print(conta.saldo)    # 1000

print(conta.depositar(500))  # Depósito de R$500.00 realizado com sucesso. Saldo atual: R$1500.00
print(conta.sacar(300))      # Saque de R$300.00 realizado com sucesso. Saldo atual: R$1200.00

# Tentativa de acesso direto (não funcionará)
# conta.__saldo = 9999  # Não modifica o saldo real

# Verificando o extrato
for op in conta.extrato:
    print(f"{op['data']} - {op['tipo']} de R${op['valor']:.2f} - Saldo: R${op['saldo_resultante']:.2f}")
```

---

## 8. Melhores Práticas

1. **Prefira Properties**: Use `@property` em vez de getters/setters tradicionais.
2. **Proteja seus dados**: Use name mangling (`__atributo`) para dados sensíveis.
3. **Comunique intenções**: Use single underscore (`_metodo`) para indicar "use com cuidado".
4. **Defina interface clara**: Exponha apenas o que é necessário.
5. **Valide dados**: Sempre verifique entradas antes de modificar estado interno.

---

