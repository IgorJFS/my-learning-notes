O que é MySQL?
**MySQL** é um sistema de gerenciamento de banco de dados relacional (RDBMS) open-source, baseado em SQL (Structured Query Language).  

Criado nos anos 90, é amplamente usado em aplicações web (ex.: WordPress, PHP).  

Características:  
Rápido e confiável.  

Suporta tabelas relacionais (chaves primárias, estrangeiras).  

Multiplataforma (Windows, Linux, etc.).

Instalação
**Windows:**  
Baixe em mysql.com/downloads.  

Instale o MySQL Server e o Workbench (interface gráfica).  

Rode: `mysql -u root -p` e digite a senha inicial.

**Linux (Ubuntu):**
```bash
sudo apt update
sudo apt install mysql-server
sudo mysql_secure_installation  # Configura senha e segurança
sudo systemctl start mysql
sudo systemctl enable mysql
```  
Acesse: `mysql -u root -p`.

Conceitos Básicos
**Banco de Dados:** Um "container" pra suas tabelas.  

**Tabela:** Estrutura com colunas e linhas, como uma planilha.  

**Colunas:** Definidas com tipos (ex.: INT, VARCHAR).  

**Linhas:** Dados reais inseridos nas colunas.

1. Criando um Banco de Dados
```sql
CREATE DATABASE minha_loja;
USE minha_loja;
```  
`CREATE DATABASE`: Cria o banco.  

`USE`: Seleciona ele pra trabalhar.

2. Criando Tabelas
Exemplo: Tabela de clientes.
```sql
CREATE TABLE clientes (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    data_cadastro DATE DEFAULT CURRENT_DATE
);
```  
`id`: Chave primária, autoincrementa (1, 2, 3...).  

`nome`: Texto até 100 caracteres, obrigatório.  

`email`: Único (não repete).  

`data_cadastro`: Data padrão é hoje.

3. Inserindo Dados
```sql
INSERT INTO clientes (nome, email)
VALUES
    ('Igor Silva', 'igor@email.com'),
    ('Ana Souza', 'ana@email.com');
```  
Insere 2 clientes. O `id` e `data_cadastro` são automáticos.

4. Consultando Dados
```sql
SELECT * FROM clientes;  -- Tudo
SELECT nome, email FROM clientes WHERE id = 1;  -- Filtrado
SELECT COUNT(*) as total FROM clientes;  -- Conta linhas
```  
`*`: Todas as colunas.  

`WHERE`: Filtra.  

`as`: Renomeia o resultado.

5. Atualizando Dados
```sql
UPDATE clientes
SET email = 'igor.novo@email.com'
WHERE id = 1;
```  
Altera o email do cliente com `id = 1`.

6. Deletando Dados
```sql
DELETE FROM clientes WHERE id = 2;  -- Deleta Ana
DROP TABLE clientes;  -- Apaga a tabela
DROP DATABASE minha_loja;  -- Apaga o banco
```  
`DELETE`: Remove linhas específicas.  

`DROP`: Destrói tabelas ou bancos.

Relacionamentos
Vamos criar um cenário real: uma loja com **clientes** e **pedidos**.  
Tabela de Pedidos
```sql
CREATE TABLE pedidos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    cliente_id INT,
    valor_total DECIMAL(10, 2),
    data_pedido DATE,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);
```  
`cliente_id`: Chave estrangeira, liga ao `id` de `clientes`.  

`DECIMAL(10, 2)`: Número com até 10 dígitos, 2 após o decimal (ex.: 12345678.90).

Inserindo Pedidos
```sql
INSERT INTO pedidos (cliente_id, valor_total, data_pedido)
VALUES
    (1, 150.50, '2025-04-10'),
    (1, 99.99, '2025-04-11');
```  
Joins
**INNER JOIN:** Combina só linhas com correspondência.
```sql
SELECT c.nome, p.valor_total
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id;
```  

Resultado: Mostra nome do cliente e valor dos pedidos dele.  

**LEFT JOIN:** Inclui todos os clientes, mesmo sem pedidos.
```sql
SELECT c.nome, COUNT(p.id) as total_pedidos
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.nome;
```  

`GROUP BY`: Agrupa por nome.  

`COUNT`: Conta pedidos por cliente.

Índices
**Índices** aceleram consultas, mas tornam inserções mais lentas.
```sql
CREATE INDEX idx_nome ON clientes(nome);
```  

Cria índice na coluna `nome` pra buscas rápidas.  

Índice único (já usado em `email`):
```sql
ALTER TABLE clientes ADD UNIQUE (email);
```

Transações
**Transações** garantem que um grupo de comandos seja todo executado ou nenhum.
```sql
START TRANSACTION;

INSERT INTO clientes (nome, email) VALUES ('João', 'joao@email.com');
INSERT INTO pedidos (cliente_id, valor_total)
VALUES (LAST_INSERT_ID(), 200.00);  
-- Se der erro, desfaz tudo
-- ROLLBACK;  
-- Se der certo, confirma
COMMIT;
```  
`LAST_INSERT_ID()`: Pega o `id` do último cliente inserido.

Funções Úteis
**String:**
```sql
SELECT UPPER(nome) as nome_maiusculo FROM clientes;  -- "IGOR SILVA"
SELECT CONCAT(nome, ' - ', email) as info FROM clientes;  -- "Igor Silva - igor@email.com"
```  

**Data:**
```sql
SELECT NOW();  -- Data e hora atual
SELECT DATEDIFF('2025-04-10', '2025-01-01') as dias;  -- Diferença em dias
```  

**Agregação:**
```sql
SELECT SUM(valor_total) as total_vendas FROM pedidos;  -- Soma valores
SELECT AVG(valor_total) as media FROM pedidos;  -- Média
```

Exemplo Avançado: Loja Completa
Tabelas
```sql
CREATE TABLE categorias (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(50)
);  
CREATE TABLE produtos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(100),
    preco DECIMAL(10, 2),
    categoria_id INT,
    FOREIGN KEY (categoria_id) REFERENCES categorias(id)
);  
CREATE TABLE itens_pedido (
    id INT AUTO_INCREMENT PRIMARY KEY,
    pedido_id INT,
    produto_id INT,
    quantidade INT,
    FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
    FOREIGN KEY (produto_id) REFERENCES produtos(id)
);
```  
Inserindo Dados
```sql
INSERT INTO categorias (nome) VALUES ('Eletrônicos'), ('Roupas');  
INSERT INTO produtos (nome, preco, categoria_id)
VALUES
    ('Smartphone', 999.99, 1),
    ('Camiseta', 29.90, 2);  
INSERT INTO pedidos (cliente_id, valor_total) VALUES (1, 1029.89);
INSERT INTO itens_pedido (pedido_id, produto_id, quantidade)
VALUES
    (1, 1, 1),  -- 1 Smartphone
    (1, 2, 1);  -- 1 Camiseta
```  
Consulta Completa
```sql
SELECT
    c.nome AS cliente,
    p.data_pedido,
    prod.nome AS produto,
    cat.nome AS categoria,
    ip.quantidade,
    (prod.preco * ip.quantidade) AS subtotal
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id
INNER JOIN itens_pedido ip ON p.id = ip.pedido_id
INNER JOIN produtos prod ON ip.produto_id = prod.id
INNER JOIN categorias cat ON prod.categoria_id = cat.id
WHERE p.data_pedido >= '2025-01-01';
```  
Resultado: Lista detalhada de pedidos com cliente, produto, categoria e subtotal.

Dicas Práticas
**Backup:**
```sql
mysqldump -u root -p minha_loja > backup.sql
```  

**Restauração:**
```sql
mysql -u root -p minha_loja < backup.sql
```  

**Ver Estrutura:**
```sql
DESCRIBE clientes;
SHOW CREATE TABLE clientes;
```

Notas de Aprendizado
MySQL é simples pra começar, mas poderoso com joins e transações.  

Sempre use índices em colunas muito filtradas (ex.: `WHERE`, `JOIN`).  

