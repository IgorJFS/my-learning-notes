# GraphQL

GraphQL é uma linguagem de consulta e manipulação de dados para APIs, criada pelo Facebook em 2012. É uma alternativa moderna ao REST que oferece maior flexibilidade e eficiência na comunicação entre cliente e servidor.

---

## 1. O que é GraphQL?

- Uma linguagem de consulta que permite ao cliente especificar exatamente quais dados deseja receber.
- Um runtime para executar essas consultas usando um sistema de tipos definido para os dados.
- Uma única URL/endpoint para todas as operações (diferente do REST que tem múltiplos endpoints).

---

## 2. Importância do GraphQL

- **Eficiência**: Elimina over-fetching (buscar dados desnecessários) e under-fetching (precisar de múltiplas requisições).
- **Flexibilidade**: O cliente define a estrutura da resposta.
- **Evolução da API**: Adicionar novos campos sem quebrar clientes existentes.
- **Documentação automática**: Schema autodocumentado.
- **Strongly typed**: Sistema de tipos robusto.

---

## 3. Quando usar GraphQL ao invés de REST?

### Vantagens do GraphQL:
- **Aplicações mobile**: Reduz uso de dados e bateria.
- **Múltiplos clientes**: Diferentes clientes podem pedir diferentes dados.
- **Desenvolvimento rápido**: Frontend e backend podem evoluir independentemente.
- **Consultas complexas**: Quando precisar de dados relacionados em uma única requisição.

### Quando REST ainda é melhor:
- **APIs simples**: Para operações CRUD básicas.
- **Cache HTTP**: REST aproveita melhor o cache HTTP.
- **Uploads de arquivo**: Mais direto no REST.
- **Equipe sem experiência**: REST tem curva de aprendizado menor.

---

## 4. Exemplo com Node.js

```js
// Instalação: npm install apollo-server-express graphql

const { ApolloServer, gql } = require('apollo-server-express');
const express = require('express');

// Schema
const typeDefs = gql`
  type User {
    id: ID!
    name: String!
    email: String!
    posts: [Post!]!
  }

  type Post {
    id: ID!
    title: String!
    content: String!
    author: User!
  }

  type Query {
    users: [User!]!
    user(id: ID!): User
    posts: [Post!]!
  }

  type Mutation {
    createUser(name: String!, email: String!): User!
    createPost(title: String!, content: String!, authorId: ID!): Post!
  }
`;

// Dados mockados
const users = [
  { id: '1', name: 'João', email: 'joao@email.com' },
  { id: '2', name: 'Maria', email: 'maria@email.com' }
];

const posts = [
  { id: '1', title: 'Meu primeiro post', content: 'Conteúdo...', authorId: '1' }
];

// Resolvers
const resolvers = {
  Query: {
    users: () => users,
    user: (_, { id }) => users.find(user => user.id === id),
    posts: () => posts
  },
  
  Mutation: {
    createUser: (_, { name, email }) => {
      const newUser = { id: String(users.length + 1), name, email };
      users.push(newUser);
      return newUser;
    },
    
    createPost: (_, { title, content, authorId }) => {
      const newPost = { 
        id: String(posts.length + 1), 
        title, 
        content, 
        authorId 
      };
      posts.push(newPost);
      return newPost;
    }
  },
  
  User: {
    posts: (user) => posts.filter(post => post.authorId === user.id)
  },
  
  Post: {
    author: (post) => users.find(user => user.id === post.authorId)
  }
};

async function startServer() {
  const app = express();
  const server = new ApolloServer({ typeDefs, resolvers });
  
  await server.start();
  server.applyMiddleware({ app });
  
  app.listen(4000, () => {
    console.log(`🚀 Servidor rodando em http://localhost:4000${server.graphqlPath}`);
  });
}

startServer();
```

### Exemplo de Query:
```graphql
query {
  users {
    id
    name
    posts {
      title
    }
  }
}
```

---

## 5. Exemplo com Python

```python
# Instalação: pip install graphene flask flask-graphql

from graphene import ObjectType, String, Int, List, Schema, Field, Mutation
from flask import Flask
from flask_graphql import GraphQLView

# Modelos
class User:
    def __init__(self, id, name, email):
        self.id = id
        self.name = name
        self.email = email

class Post:
    def __init__(self, id, title, content, author_id):
        self.id = id
        self.title = title
        self.content = content
        self.author_id = author_id

# Dados mockados
users = [
    User(1, "João", "joao@email.com"),
    User(2, "Maria", "maria@email.com")
]

posts = [
    Post(1, "Meu primeiro post", "Conteúdo...", 1)
]

# Types
class UserType(ObjectType):
    id = Int()
    name = String()
    email = String()
    posts = List(lambda: PostType)
    
    def resolve_posts(self, info):
        return [post for post in posts if post.author_id == self.id]

class PostType(ObjectType):
    id = Int()
    title = String()
    content = String()
    author = Field(UserType)
    
    def resolve_author(self, info):
        return next((user for user in users if user.id == self.author_id), None)

# Queries
class Query(ObjectType):
    users = List(UserType)
    user = Field(UserType, id=Int(required=True))
    posts = List(PostType)
    
    def resolve_users(self, info):
        return users
    
    def resolve_user(self, info, id):
        return next((user for user in users if user.id == id), None)
    
    def resolve_posts(self, info):
        return posts

# Mutations
class CreateUser(Mutation):
    class Arguments:
        name = String(required=True)
        email = String(required=True)
    
    user = Field(UserType)
    
    def mutate(self, info, name, email):
        new_user = User(len(users) + 1, name, email)
        users.append(new_user)
        return CreateUser(user=new_user)

class Mutations(ObjectType):
    create_user = CreateUser.Field()

# Schema
schema = Schema(query=Query, mutation=Mutations)

# Flask App
app = Flask(__name__)
app.add_url_rule('/graphql', view_func=GraphQLView.as_view('graphql', schema=schema, graphiql=True))

if __name__ == '__main__':
    app.run(debug=True)
```

---

## 6. Conceitos Importantes

- **Schema**: Define a estrutura dos dados e operações disponíveis.
- **Resolvers**: Funções que buscam os dados para cada campo.
- **Queries**: Operações de leitura.
- **Mutations**: Operações de escrita (criar, atualizar, deletar).
- **Subscriptions**: Para dados em tempo real.

---

## 7. Ferramentas Úteis

- **GraphiQL**: Interface gráfica para testar queries.
- **Apollo Studio**: Platform para desenvolvimento GraphQL.
- **Postman**: Suporte para GraphQL.

---
