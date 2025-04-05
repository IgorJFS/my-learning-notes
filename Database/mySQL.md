Maneira codificada (para copiar no VS Code)
# O que é SQL? Sintaxe Básica com Exemplos
**SQL** (Structured Query Language) é a linguagem usada pra gerenciar bancos de dados relacionais, como o MySQL. É como um "garçom" que você usa pra pedir, adicionar ou mudar dados. Aqui vamos cobrir os conceitos básicos e avançados com exemplos.
## O que é SQL?
SQL é usado pra:
Criar e organizar tabelas.

Inserir, buscar, atualizar e deletar dados.

Relacionar informações entre tabelas.

No MySQL, é essencial pra projetos como sites ou apps.
## Sintaxe Básica e Exemplos
### 1. CREATE TABLE - Criar uma tabela
Define a estrutura dos dados.
```sql
CREATE TABLE usuarios (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nome VARCHAR(50),
  email VARCHAR(100)
);
```
### 2. INSERT - Adicionar dados
Insere novas linhas.
```sql
INSERT INTO usuarios (nome, email) 
VALUES ('Igor', 'igor@email.com');
```
### 3. SELECT - Buscar dados
Recupera informações.
```sql
SELECT nome, email FROM usuarios WHERE id = 1;
```
Busca nome e email do usuário com id 1.

```sql
SELECT * FROM usuarios;
```
Pega todos os dados da tabela.

### 4. WHERE - Filtrar resultados
Especifica condições no SELECT, UPDATE ou DELETE.
```sql
SELECT * FROM usuarios WHERE nome = 'Igor';
```
Mostra só usuários chamados "Igor".

#### Operadores do WHERE
= (igual), != (diferente), <, >, <=, >=.

LIKE: Busca padrões (ex.: %igor% pra nomes com "igor").

IN: Verifica numa lista.

BETWEEN: Intervalo.

```sql
SELECT * FROM usuarios 
WHERE id BETWEEN 1 AND 5 
AND email LIKE '%@email
.com';
```
Pega usuários com id de 1 a 5 e email terminando em "@email
.com".

### 5. JOIN - Juntar tabelas
Combina dados de várias tabelas usando chaves.
#### Tipos de JOIN
**INNER JOIN**: Só linhas que combinam.

**LEFT JOIN**: Tudo da tabela à esquerda, mesmo sem combinação.

**RIGHT JOIN**: Tudo da tabela à direita.

```sql
CREATE TABLE pedidos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  usuario_id INT,
  produto VARCHAR(50)
);
INSERT INTO pedidos (usuario_id, produto) 
VALUES (1, 'Mouse');
SELECT u.nome, p.produto 
FROM usuarios u 
INNER JOIN pedidos p ON u.id = p.usuario_id;
```
Mostra o nome do usuário e o produto que ele pediu.

### 6. FOREIGN KEY - Relacionar tabelas
Garante que uma coluna aponte pra uma chave primária em outra tabela.
```sql
CREATE TABLE pedidos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  usuario_id INT,
  produto VARCHAR(50),
  FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);
```
usuario_id em pedidos só aceita valores que existam em usuarios(id).

### 7. ORDER BY - Ordenar resultados
Organiza os dados.
```sql
SELECT * FROM usuarios 
ORDER BY nome ASC;
```
Lista usuários em ordem alfabética (ASC = crescente, DESC = decrescente).

### 8. LIMIT e OFFSET - Controlar quantidade
LIMIT define quantos resultados, OFFSET pula linhas.
```sql
SELECT * FROM usuarios 
ORDER BY id 
LIMIT 2 OFFSET 1;
```
Pega 2 usuários, pulando o primeiro (mostra o 2º e 3º).

### 9. AS - Renomear colunas ou tabelas
Dá apelidos pra facilitar.
```sql
SELECT nome AS nome_usuario, email AS correo 
FROM usuarios AS u;
```
Renomeia nome pra "nome_usuario" e email pra "correo", apelidando a tabela como u.

### 10. UPDATE - Atualizar dados
Modifica linhas.
```sql
UPDATE usuarios 
SET email = 'novo@email.com' 
WHERE id = 1;
```
### 11. DELETE - Deletar dados
Remove linhas.
```sql
DELETE FROM usuarios 
WHERE id = 1;
```
### 12. ALTER TABLE - Modificar estrutura
Adiciona ou muda colunas.
```sql
ALTER TABLE usuarios 
ADD idade INT;
```
### 13. DROP TABLE - Deletar tabela
Remove tudo.
```sql
DROP TABLE usuarios;
```
## Exemplo completo
Criando e manipulando duas tabelas relacionadas:
```sql
-- Criar tabelas
CREATE TABLE usuarios (
  id INT AUTO_INCREMENT PRIMARY KEY,
  nome VARCHAR(50),
  email VARCHAR(100)
);
CREATE TABLE pedidos (
  id INT AUTO_INCREMENT PRIMARY KEY,
  usuario_id INT,
  produto VARCHAR(50),
  FOREIGN KEY (usuario_id) REFERENCES usuarios(id)
);
-- Inserir dados
INSERT INTO usuarios (nome, email) VALUES ('Igor', 'igor@email.com');
INSERT INTO usuarios (nome, email) VALUES ('Ana', 'ana@email.com');
INSERT INTO pedidos (usuario_id, produto) VALUES (1, 'Mouse');
INSERT INTO pedidos (usuario_id, produto) VALUES (2, 'Teclado');
-- Buscar com JOIN
SELECT u.nome AS cliente, p.produto 
FROM usuarios u 
INNER JOIN pedidos p ON u.id = p.usuario_id 
WHERE p.produto LIKE '%o' 
ORDER BY u.nome 
LIMIT 1 OFFSET 0;
```
Resultado: "Igor" e "Mouse" (filtra produtos com "o", ordena por nome, pega o 1º).

## Dicas pra começar
**Tipos de dados**: INT, VARCHAR, DECIMAL, etc.

**Chave primária**: PRIMARY KEY pra unicidade.

**Teste aos poucos**: Rode cada comando pra entender o efeito.
