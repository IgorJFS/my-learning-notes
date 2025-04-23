# Guia Completo de MongoDB para Full Stack Developers

## Objetivo

Aprender MongoDB do básico ao avançado, com foco em habilidades práticas pra te preparar como full stack developer. Este guia cobre instalação, modelagem NoSQL, consultas, índices, agregações, integração com Node.js, e boas práticas, com exemplos e analogias pra tornar tudo mais fácil.

## Introdução: O que é MongoDB?

MongoDB é um banco de dados **NoSQL** orientado a documentos, open-source, projetado pra flexibilidade e rapidez. Ele armazena dados em **coleções** (como pastas) que contêm **documentos** (arquivos JSON-like). Pense no MongoDB como um **hambúrguer personalizado**: você monta os dados do jeito que quiser, com ingredientes (campos) que podem variar de um documento pra outro, sem seguir uma receita fixa.

### Por que usar MongoDB?

- **Flexibilidade**: Adicione ou mude campos sem alterar um schema rígido.
- **Rapidez**: Ideal pra aplicações com muitos dados dinâmicos (ex.: e-commerce, redes sociais).
- **Integração**: Combina bem com JavaScript e Node.js, perfeito pra full stack.

## Instalação e Configuração

### Instalando o MongoDB

No Ubuntu:

```bash
sudo apt update
sudo apt install -y mongodb
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

No macOS (via Homebrew):

```bash
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb/brew/mongodb-community
```

### Acessando o mongosh

O **mongosh** é o shell interativo do MongoDB, onde você executa comandos. Acesse assim:

```bash
mongosh
```

**Nova palavra**: **mongosh** é o cliente moderno do MongoDB, usado pra interagir com o banco.

## Fundamentos: Criando Bancos e Coleções

### Criando um Banco de Dados

No MongoDB, você cria um banco simplesmente usando ele:

```javascript
use burgershop
```

O banco **burgershop** só é criado de verdade quando você insere dados.

### Criando Coleções e Inserindo Documentos

Coleções são como pastas que guardam documentos (objetos JSON-like). Exemplo pra uma hamburgueria:

```javascript
db.customers.insertOne({
  name: "João",
  email: "joao@email.com",
  preferences: { favorite_burger: "Clássico", spicy: false }
});

db.customers.insertMany([
  { name: "Maria", email: "maria@email.com", preferences: { favorite_burger: "Vegano", spicy: true } },
  { name: "Pedro", email: "pedro@email.com", preferences: { favorite_burger: "Gourmet", spicy: false } }
]);
```

**Novas palavras**:
- **db**: Representa o banco atual no mongosh.
- **insertOne**: Insere um documento.
- **insertMany**: Insere vários documentos.
- **Documentos**: Objetos JSON-like (em formato BSON) que guardam os dados.

A coleção **customers** é criada automaticamente ao inserir o primeiro documento.

### Estrutura de um Documento

Um documento pode ter campos variados. Exemplo de um pedido:

```javascript
db.orders.insertOne({
  customer_id: 1,
  order_date: new Date(),
  total: 25.50,
  items: [
    { item_name: "Hambúrguer Clássico", price: 15.00 },
    { item_name: "Batata Frita", price: 10.50 }
  ]
});
```

**Dica**: Documentos podem ter objetos aninhados (como **items**) e arrays, o que dá flexibilidade pra modelar dados.

## Consultas Básicas

### Buscando Documentos

Listar todos os clientes:

```javascript
db.customers.find()
```

Adicione **pretty()** pra formatar a saída:

```javascript
db.customers.find().pretty()
```

**Nova palavra**: **pretty()** torna o JSON mais legível.

Buscar por um campo específico:

```javascript
db.customers.find({ email: "joao@email.com" }).pretty()
```

### Usando Operadores

Exemplo com **$gte** (maior ou igual):

```javascript
db.orders.find({ total: { $gte: 20 } }).pretty()
```

**Novas palavras**:
-** **$gte**: Maior ou igual (ex.: total >= 20).
-** **$lt**: Menor que.
-** **$eq**: Igual.
-** **$in**: Verifica se um valor está numa lista.

Exemplo com **$in**:

```javascript
db.customers.find({ "preferences.favorite_burger": { $in: ["Clássico", "Vegano"] } }).pretty()
```

**Dica**: Use notação de ponto (ex.: **preferences.favorite_burger**) pra acessar campos aninhados.

## Atualizando e Removendo Documentos

### Atualizando Documentos

Atualizar o e-mail de um cliente:

```javascript
db.customers.updateOne(
  { email: "joao@email.com" },
  { $set: { email: "joao.novo@email.com" } }
);
```

**Novas palavras**:
-** **updateOne**: Atualiza um documento.
-** **$set**: Define novos valores pra campos.

Atualizar vários documentos:

```javascript
db.customers.updateMany(
  { "preferences.spicy": true },
  { $set: { "preferences.discount": 0.1 } }
);
```

### Removendo Documentos

Remover um cliente:

```javascript
db.customers.deleteOne({ email: "joao.novo@email.com" });
```

Remover vários:

```javascript
db.customers.deleteMany({ "preferences.favorite_burger": "Gourmet" });
```

**Novas palavras**:
-** **deleteOne**: Remove um documento.
-** **deleteMany**: Remove vários documentos.

## Recursos Avançados para Full Stack

### Índices

Índices aceleram consultas. Criar um índice no campo **email**:

```javascript
db.customers.createIndex({ email: 1 });
```

**Nova palavra**: **createIndex** cria índices (1 = ascendente, -1 = descendente).

Índice composto pra pedidos:

```javascript
db.orders.createIndex({ customer_id: 1, order_date: -1 });
```

**Dica**: Índices são cruciais pra consultas frequentes, mas evite criar muitos, pois aumentam o tempo de escrita.

### Agregações

Agregações processam dados em estágios (como uma linha de montagem de hambúrgueres). Exemplo: somar o total de pedidos por cliente:

```javascript
db.orders.aggregate([
  { $group: { _id: "$customer_id", total_spent: { $sum: "$total" } } }
]).pretty();
```

**Novas palavras**:
-** **aggregate**: Executa um pipeline de agregação.
-** **$group**: Agrupa documentos por um campo.
-** **$sum**: Soma valores.

Exemplo mais complexo, com filtro e projeção:

```javascript
db.orders.aggregate([
  { $match: { total: { $gte: 20 } } },
  { $group: { _id: "$customer_id", total_spent: { $sum: "$total" } } },
  { $project: { customer_id: "$_id", total_spent: 1, _id: 0 } }
]).pretty();
```

**Novas palavras**:
-** **$match**: Filtra documentos.
-** **$project**: Seleciona campos pra incluir/excluir.

### Transações

MongoDB suporta transações desde a v4.0, úteis pra operações atômicas. Exemplo:

```javascript
const session = db.getMongo().startSession();
session.startTransaction();
try {
  db.orders.insertOne(
    { customer_id: 1, total: 30.00, items: [{ item_name: "Hambúrguer Gourmet", price: 30.00 }] },
    { session }
  );
  db.customers.updateOne(
    { _id: 1 },
    { $set: { last_order: new Date() } },
    { session }
  );
  session.commitTransaction();
} catch (error) {
  session.abortTransaction();
  console.error(error);
}
session.endSession();
```

**Dica**: Use transações só quando precisar garantir consistência entre múltiplas operações.

## Integração com Node.js

Como full stack, você vai conectar o MongoDB ao back-end. Exemplo com a biblioteca **mongodb**:

```javascript
const { MongoClient } = require('mongodb');

async function main() {
  const uri = "mongodb://localhost:27017";
  const client = new MongoClient(uri);
  try {
    await client.connect();
    const db = client.db('burgershop');
    const orders = db.collection('orders');
    const highValueOrders = await orders.find({ total: { $gte: 20 } }).toArray();
    console.log(highValueOrders);
  } catch (err) {
    console.error(err);
  } finally {
    await client.close();
  }
}

main();
```

**Dica**: O driver **mongodb** é nativo pra JavaScript, facilitando integração com Node.js.

## Boas Práticas para Full Stack

- **Modelagem flexível**: Aninhe dados relacionados (ex.: itens dentro de pedidos), mas evite documentos muito grandes.
- **Índices estratégicos**: Crie índices pra campos usados em filtros ou ordenações.
- **Use transações com moderação**: Só pra operações que exigem consistência.
- **Segurança**: Sempre valide dados no back-end e habilite autenticação no MongoDB.
- **Backups**: Use **mongodump** pra exportar dados:

```bash
mongodump --db burgershop --out burgershop_backup
```

## Quando usar MongoDB?

- **Aplicações dinâmicas**: Quando os dados mudam frequentemente (ex.: catálogos de produtos).
- **Prototipagem rápida**: Pra criar MVPs sem definir schemas.
- **Integração com JavaScript**: Perfeito pra stacks com Node.js.
