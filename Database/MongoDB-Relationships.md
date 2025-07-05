# Relacionamentos de Dados com MongoDB e Integração com Express

## 1. O que são Relacionamentos em MongoDB?

MongoDB é um banco de dados NoSQL orientado a documentos, onde os dados são armazenados em documentos JSON flexíveis. Diferente de bancos relacionais, não há JOINs nativos, mas é possível modelar relações entre dados de duas formas principais: **Documentos Embutidos** e **Referências**.

---

### a) Documentos Embutidos (Embedded Documents)

**Definição:**

- Consiste em armazenar documentos relacionados dentro de um mesmo documento principal.
- É útil quando os dados têm forte ligação e geralmente são acessados juntos.

**Vantagens:**

- Leitura mais rápida (tudo em um único documento).
- Simplicidade para dados que não crescem muito.

**Desvantagens:**

- Pode causar duplicação de dados.
- Dificulta atualizações se o dado embutido for compartilhado por vários documentos.

**Exemplo:**
Um pedido com seus itens embutidos:

```json
{
  "_id": 1,
  "cliente": "João",
  "itens": [
    { "produto": "Caneta", "quantidade": 2 },
    { "produto": "Caderno", "quantidade": 1 }
  ]
}
```

---

### b) Referências (References)

**Definição:**

- Em vez de embutir, você armazena apenas o ID do documento relacionado.
- Os dados ficam separados, mas conectados por referência.

**Vantagens:**

- Evita duplicação de dados.
- Permite que documentos cresçam de forma independente.

**Desvantagens:**

- Requer múltiplas consultas para "juntar" os dados (ou uso de ferramentas como o populate do Mongoose).
- Pode ser mais lento para leitura, mas facilita atualizações.

**Exemplo:**
Pedido referenciando produtos:

```json
// Pedido
{
  "_id": 1,
  "cliente": "João",
  "itens": [
    { "produtoId": ObjectId("abc123"), "quantidade": 2 }
  ]
}

// Produto
{
  "_id": ObjectId("abc123"),
  "nome": "Caneta"
}
```

---

## 2. Relacionamentos com Express.js e Mongoose

Quando você usa Express para criar APIs e Mongoose para modelar dados, pode lidar com relacionamentos de duas formas:

### a) Usando Referências e o `.populate()`

O método `.populate()` do Mongoose permite buscar automaticamente os dados referenciados, simulando um JOIN.

```js
// Schema Produto
const ProdutoSchema = new mongoose.Schema({ nome: String });
const Produto = mongoose.model("Produto", ProdutoSchema);

// Schema Pedido
const PedidoSchema = new mongoose.Schema({
  cliente: String,
  itens: [
    {
      produto: { type: mongoose.Schema.Types.ObjectId, ref: "Produto" },
      quantidade: Number,
    },
  ],
});
const Pedido = mongoose.model("Pedido", PedidoSchema);

// Buscar pedido com produtos populados
Pedido.find()
  .populate("itens.produto")
  .then((pedidos) => {
    res.json(pedidos);
  });
```

### b) Usando Embedded Documents

Quando os dados estão embutidos, basta consultar o documento principal e acessar os dados normalmente, sem necessidade de populate.

```js
// Exemplo de schema com embedded documents
const PedidoSchema = new mongoose.Schema({
  cliente: String,
  itens: [
    {
      produto: String, // nome do produto
      quantidade: Number,
    },
  ],
});
```

---

## 3. Quando usar cada abordagem?

- **Embedded:**
  - Dados pequenos, fortemente relacionados, que raramente mudam sozinhos.
  - Exemplo: endereços de entrega de um usuário, comentários de um post.
- **Referências:**
  - Dados grandes, independentes, ou compartilhados entre vários documentos.
  - Exemplo: produtos em pedidos, autores de livros.

**Dica:** Sempre pense em como os dados serão acessados na sua aplicação. Se quase sempre você precisa dos dados juntos, embedded pode ser melhor. Se os dados mudam muito ou são compartilhados, prefira referências.

---

**Referências:**

- [Documentação MongoDB - Data Modeling](https://www.mongodb.com/docs/manual/core/data-modeling-introduction/)
- [Mongoose Populate](https://mongoosejs.com/docs/populate.html)
