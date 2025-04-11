# O que é MySQL?
```MySQL``` é um sistema de gerenciamento de banco de dados relacional (RDBMS) open-source, baseado em SQL (Structured Query Language).  

Criado nos anos 90, é amplamente usado em aplicações web (ex.: WordPress, PHP).  

Características:  
Rápido e confiável.  

Suporta tabelas relacionais (chaves primárias, estrangeiras).  

Multiplataforma (Windows, Linux, etc.).

Conceitos Básicos
**Banco de Dados:** Um "container" pra suas tabelas.  

**Tabela:** Estrutura com colunas e linhas, como uma planilha.  

**Colunas:** Definidas com tipos (ex.: INT, VARCHAR).  

**Linhas:** Dados reais inseridos nas colunas.

1. **Criando um Banco de Dados**
```sql
CREATE DATABASE minha_loja;
USE minha_loja;
```  
`CREATE DATABASE`: Cria o banco.  

`USE`: Seleciona ele pra trabalhar.

2. **Criando Tabelas**
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

3. **Inserindo Dados**
```sql
INSERT INTO clientes (nome, email)
VALUES
    ('Igor Silva', 'igor@email.com'),
    ('Ana Souza', 'ana@email.com');
```  
Insere 2 clientes. O `id` e `data_cadastro` são automáticos.

4. **Consultando Dados**
```sql
SELECT * FROM clientes;  -- Tudo
SELECT nome, email FROM clientes WHERE id = 1;  -- Filtrado
SELECT COUNT(*) as total FROM clientes;  -- Conta linhas
```  
`*`: Todas as colunas.  

`WHERE`: Filtra.  

`as`: Renomeia o resultado.

5. **Atualizando Dados**
```sql
UPDATE clientes
SET email = 'igor.novo@email.com'
WHERE id = 1;
```  
Altera o email do cliente com `id = 1`.

6. **Deletando Dados**
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

## Funções Úteis
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

**CAST:**  
Converte um valor de um tipo pra outro.  

Exemplo: Transforma string em inteiro.
```sql
SELECT CAST('123' AS UNSIGNED) as numero;  -- Converte "123" pra 123
SELECT CAST(15.75 AS CHAR) as texto;  -- Converte 15.75 pra "15.75"
```

**CONVERT:**  
Similar ao CAST, mas com suporte a conversão de caracteres (ex.: charset).  

Exemplo: Converte data pra string ou ajusta codificação.
```sql
SELECT CONVERT('2025-04-10', CHAR) as data_texto;  -- "2025-04-10" como texto
SELECT CONVERT(500, DECIMAL(5,2)) as decimal_valor;  -- 500.00
```

**STR_TO_DATE:**  
Transforma uma string em data, especificando o formato.  

Exemplo: Converte "10/04/2025" pra data MySQL.
```sql
SELECT STR_TO_DATE('10/04/2025', '%d/%m/%Y') as data_formatada;  -- 2025-04-10
SELECT STR_TO_DATE('2025-04-10 14:30', '%Y-%m-%d %H:%i') as data_hora;
```  

Formatos: `%d` = dia, `%m` = mês, `%Y` = ano 4 dígitos, `%H` = hora, `%i` = minutos.


## Operações de Conjunto
MySQL suporta operações pra combinar ou comparar resultados de várias consultas.  

**Nota:** Nem todas (ex.: EXCEPT, INTERSECT) são nativas no MySQL, mas vou mostrar alternativas.  

**UNION:**  
Combina resultados de duas ou mais consultas, removendo duplicatas.  

As colunas devem ter o mesmo número e tipos compatíveis.  

Exemplo: Lista clientes e produtos numa única coluna.
```sql
SELECT nome FROM clientes
UNION
SELECT nome FROM produtos;
```  

Resultado: "Igor Silva", "Ana Souza", "Smartphone", "Camiseta" (sem duplicatas).

**UNION ALL:**  
Igual ao UNION, mas mantém duplicatas.  

Mais rápido por não verificar duplicatas.  

Exemplo: Clientes e produtos, permitindo repetições.
```sql
SELECT nome FROM clientes
UNION ALL
SELECT nome FROM produtos;
```  

Resultado: Pode repetir "Igor Silva" se aparecer em ambas as tabelas.

**EXCEPT:**  
Retorna linhas da primeira consulta que não estão na segunda.  

**Atenção:** MySQL não suporta EXCEPT nativo, mas usamos LEFT JOIN ou NOT IN.  

Exemplo: Clientes sem pedidos.
```sql
SELECT c.nome
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id
WHERE p.id IS NULL;
```  

Alternativa com NOT IN:
```sql
SELECT nome FROM clientes
WHERE id NOT IN (SELECT cliente_id FROM pedidos WHERE cliente_id IS NOT NULL);
```  

Resultado: Só clientes que nunca compraram.

**INTERSECT:**  
Retorna linhas comuns às duas consultas.  

**Atenção:** MySQL não suporta INTERSECT nativo, mas usamos INNER JOIN ou IN.  

Exemplo: Clientes que também têm pedidos.
```sql
SELECT DISTINCT c.nome
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id;
```  

Alternativa com IN:
```sql
SELECT nome FROM clientes
WHERE id IN (SELECT cliente_id FROM pedidos);
```  

Resultado: Só clientes com pedidos (ex.: "Igor Silva").

Exemplo Combinado
Cenário: Queremos uma lista única de nomes (clientes e produtos), depois clientes sem pedidos.
```sql
-- UNION pra nomes
SELECT nome AS item FROM clientes
UNION
SELECT nome FROM produtos;

-- EXCEPT simulado pra clientes sem pedidos
SELECT c.nome
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id
WHERE p.id IS NULL;
`




## Exemplo Avançado: Loja Completa
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

## Declaração de Variável

Declaração de Variável
**Variáveis** no MySQL permitem armazenar valores temporários em sessões ou scripts.  

Usadas com `SET` ou `DECLARE` (em procedures).  

Exemplo básico com `SET`:
```sql
SET @usuario_id
 = 1;
SELECT * FROM clientes WHERE id = @usuario_id
;  -- Usa a variável
```  

Exemplo com cálculo:
```sql
SET @desconto
 = 0.10;
SELECT valor_total, (valor_total * (1 - @desconto
)) as valor_descontado
FROM pedidos;
```  

Exemplo em stored procedure com `DECLARE`:
```sql
DELIMITER //
CREATE PROCEDURE get_total_vendas()
BEGIN
DECLARE total DECIMAL(10,2);
SET total = (SELECT SUM(valor_total) FROM pedidos);
SELECT total AS vendas_totais;
END //
DELIMITER ;
CALL get_total_vendas();
```  

**Nota:** `@variavel
` é global na sessão, `DECLARE` é local ao bloco.



Notas de Aprendizado
MySQL é simples pra começar, mas poderoso com joins e transações.  

Sempre use índices em colunas muito filtradas (ex.: `WHERE`, `JOIN`).  

