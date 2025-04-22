# Guia Completo de PostgreSQL para Full Stack Developers

## Objetivo

Aprender PostgreSQL do básico ao avançado, com foco em diferenças do MySQL, pra te preparar como full stack developer. Este guia cobre instalação, modelagem, consultas, índices, transações, JSONB, integração com Node.js, e boas práticas, com analogias pra facilitar. Se você já sabe MySQL, vou destacar palavras novas e sintaxes específicas do PostgreSQL.

## Introdução: O que é PostgreSQL?

PostgreSQL (ou *Postgres*) é um banco de dados relacional open-source, conhecido por sua robustez, conformidade com padrões SQL, e recursos avançados (ex.: JSONB, extensões). Pense no Postgres como um *hambúrguer gourmet*: enquanto o MySQL é um hambúrguer clássico (rápido, confiável), o Postgres adiciona ingredientes sofisticados (ex.: suporte a JSON, transações complexas, extensibilidade).

### Diferenças principais vs. MySQL

-/*/ *Conformidade SQL*: Postgres segue o padrão SQL mais rigorosamente; MySQL é mais permissivo.
-/*/ *Tipos de dados*: Postgres tem tipos avançados (JSONB, arrays, UUID); MySQL é mais básico.
-/*/ *Transações*: Postgres suporta transações completas com *SAVEPOINT* ; MySQL (com InnoDB) é menos flexível.
-/*/ *Índices*: Postgres tem índices GiST, GIN, BRIN; MySQL foca em B-tree.
-/*/ *Extensões*: Postgres permite extensões (ex.: PostGIS); MySQL não tem equivalente.
-/*/ *Case sensitivity*: No Postgres, nomes de tabelas/colunas são *case-sensitive* se não estiverem entre aspas; MySQL varia por sistema operacional.

## Instalação e Configuração

### Instalando o PostgreSQL

No Ubuntu:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

No macOS (via Homebrew):

```bash
brew install postgresql
brew services start postgresql
```

*Diferença do MySQL*: No MySQL, você usa `mysql -u root -p` pra acessar; no Postgres, o usuário padrão é `postgres`, e você usa `psql -U postgres`.

### Acessando o psql

```bash
sudo -u postgres psql
```

No *psql* (equivalente ao CLI do MySQL), você executa comandos SQL e meta-comandos (ex.: `\l` pra listar bancos). *Nova palavra*: `psql` é o cliente interativo do Postgres, diferente do `mysql` CLI.

## Fundamentos: Criando Bancos e Tabelas

### Criando um Banco de Dados

```sql
CREATE DATABASE burgershop;
```

Conectar ao banco:

```bash
psql -U postgres -d burgershop
```

*Diferença do MySQL*: A sintaxe é igual (`CREATE DATABASE`), mas no Postgres você usa `\c burgershop` no psql pra mudar de banco, em vez de `USE burgershop`.

### Criando Tabelas

Exemplo de tabelas pra uma hamburgueria:

```sql
CREATE TABLE customers (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    customer_id INTEGER REFERENCES customers(id),
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    total DECIMAL(10, 2)
);

CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INTEGER REFERENCES orders(id),
    item_name VARCHAR(100),
    price DECIMAL(10, 2)
);
```

*Novas palavras*:
-/*/ `SERIAL`: Tipo de dado auto-incrementável (equivalente ao `AUTO_INCREMENT` do MySQL).
-/*/ `TIMESTAMP`: Armazena data/hora (como `DATETIME` no MySQL, mas mais preciso).
-/*/ `REFERENCES`: Define chaves estrangeiras (igual no MySQL, mas Postgres é mais rigoroso com integridade).

*Diferença do MySQL*: No MySQL, você pode usar `INT AUTO_INCREMENT`; no Postgres, `SERIAL` é mais comum. O Postgres também exige que chaves estrangeiras sejam validadas por padrão, enquanto o MySQL (com MyISAM) pode ignorar.

## Consultas Básicas

### Inserindo Dados

```sql
INSERT INTO customers (name, email) VALUES
    ('João', 'joao@email.com'),
    ('Maria', 'maria@email.com');

INSERT INTO orders (customer_id, total) VALUES
    (1, 25.50),
    (2, 18.75);

INSERT INTO order_items (order_id, item_name, price) VALUES
    (1, 'Hambúrguer Clássico', 15.00),
    (1, 'Batata Frita', 10.50),
    (2, 'Hambúrguer Vegano', 18.75);
```

*Diferença do MySQL*: A sintaxe do `INSERT` é idêntica, mas o Postgres suporta *RETURNING* pra retornar dados inseridos (ex.: `INSERT ... RETURNING id`).

### Consultando Dados

Selecionar clientes:

```sql
SELECT * FROM customers;
```

Join com pedidos:

```sql
SELECT c.name, o.order_date, o.total
FROM customers c
JOIN orders o ON c.id = o.customer_id;
```

*Diferença do MySQL*: A sintaxe de `SELECT` e `JOIN` é a mesma, mas o Postgres tem funções agregadas mais poderosas (ex.: `json_agg`) e é mais estrito com aliases.

## Recursos Avançados para Full Stack

### Índices

Índices melhoram a performance de consultas. No Postgres, além do *B-tree* (padrão no MySQL), há *GiST*, *GIN*, e *BRIN*.

Exemplo de índice B-tree:

```sql
CREATE INDEX idx_customer_email ON customers(email);
```

*Nova palavra*: *GIN* (Generalized Inverted Index) é ideal pra JSONB ou arrays. Exemplo:

```sql
CREATE INDEX idx_order_items_gin ON order_items USING GIN(item_name);
```

*Diferença do MySQL*: MySQL usa principalmente B-tree; Postgres oferece mais opções pra casos complexos (ex.: busca em JSON).

### Transações

Transações garantem que operações sejam atômicas. No Postgres, use *BEGIN*, *COMMIT*, e *ROLLBACK* (como no MySQL), mas o Postgres suporta *SAVEPOINT* pra transações parciais.

Exemplo:

```sql
BEGIN;
INSERT INTO orders (customer_id, total) VALUES (1, 30.00);
SAVEPOINT order_saved;
INSERT INTO order_items (order_id, item_name, price) VALUES (3, 'Hambúrguer Gourmet', 30.00);
-- Erro: order_id 3 não existe
ROLLBACK TO order_saved;
COMMIT;
```

*Nova palavra*: *SAVEPOINT* permite reverter parte de uma transação, algo que o MySQL não suporta diretamente.

### JSONB

O Postgres tem *JSONB* (JSON binário), ideal pra dados semi-estruturados, algo que o MySQL suporta de forma limitada com o tipo JSON.

Exemplo: Adicionar uma coluna JSONB pra preferências do cliente:

```sql
ALTER TABLE customers ADD COLUMN preferences JSONB;
UPDATE customers SET preferences = '{"favorite_burger": "Clássico", "spicy": false}' WHERE id = 1;
```

Consultar JSONB:

```sql
SELECT name, preferences->>'favorite_burger' AS favorite
FROM customers
WHERE preferences->>'spicy' = 'false';
```

*Novas palavras*:
-/*/ *JSONB*: Tipo de dado binário pra JSON, mais eficiente que o JSON do MySQL.
-/*/ `->>`: Operador que extrai valores JSON como texto.

*Diferença do MySQL*: O JSON do MySQL é menos performático e tem menos operadores que o JSONB do Postgres.

### Funções e Triggers

Crie funções (equivalente a stored procedures no MySQL) com *CREATE FUNCTION*:

```sql
CREATE FUNCTION calculate_order_total(order_id INTEGER) RETURNS DECIMAL AS $$
BEGIN
    RETURN (SELECT SUM(price) FROM order_items WHERE order_id = calculate_order_total.order_id);
END;
$$ LANGUAGE plpgsql;
```

*Nova palavra*: *plpgsql* é a linguagem procedural do Postgres (como SQL puro, mas com lógica).

Trigger pra atualizar o total do pedido:

```sql
CREATE FUNCTION update_order_total() RETURNS TRIGGER AS $$
BEGIN
    UPDATE orders
    SET total = (SELECT calculate_order_total(NEW.order_id))
    WHERE id = NEW.order_id;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER order_items_total
AFTER INSERT ON order_items
FOR EACH ROW EXECUTE FUNCTION update_order_total();
```

*Diferença do MySQL*: No MySQL, triggers e procedures são similares, mas o Postgres tem *plpgsql* pra lógica mais complexa.

### Integração com Node.js

Como full stack, você vai conectar o Postgres ao back-end. Exemplo com *node-postgres* (biblioteca `pg`):

```javascript
const { Pool } = require('pg');

const pool = new Pool({
  user: 'postgres',
  host: 'localhost',
  database: 'burgershop',
  password: 'sua_senha',
  port: 5432,
});

async function getOrders() {
  try {
    const res = await pool.query(`
      SELECT c.name, o.order_date, o.total
      FROM customers c
      JOIN orders o ON c.id = o.customer_id
    `);
    console.log(res.rows);
  } catch (err) {
    console.error(err.stack);
  }
}

getOrders();
```

*Diferença do MySQL*: No MySQL, você usaria a biblioteca `mysql2`. A sintaxe do `pg` é similar, mas suporta recursos Postgres como JSONB.

## Boas Práticas para Full Stack

-/ *Use SERIAL pra IDs*: Evite UUIDs a menos que precise de unicidade global.
-/ *Índices com moderação*: Índices aceleram consultas, mas aumentam o tempo de escrita.
-/ *Transações sempre*: Use BEGIN/COMMIT pra operações críticas.
-/ *JSONB com cuidado*: Ideal pra dados flexíveis, mas normalize dados estruturados.
-/ *Segurança*: Use variáveis preparadas no Node.js (ex.: `pool.query('SELECT $1', [valor])`) pra evitar SQL injection.
-/ *Backups*: Use `pg_dump` pra exportar bancos:

```bash
pg_dump -U postgres burgershop > burgershop_backup.sql
```

## Quando usar PostgreSQL?

-/*/ *Projetos complexos*: Quando precisar de JSONB, transações robustas, ou extensões.
-/*/ *Aplicações full stack*: Integra bem com Node.js, Python, etc.
-/*/ *Dados geoespaciais*: Com a extensão PostGIS.

Use MySQL se precisar de simplicidade ou compatibilidade com sistemas legados.
