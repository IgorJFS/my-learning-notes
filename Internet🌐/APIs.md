# O que é uma API? Explicação com comida e introdução ao Swagger
Uma **API** (Interface de Programação de Aplicações) é como um garçom em um restaurante: ela leva seu pedido ao cozinheiro e traz a comida pronta de volta pra você. Vamos entender isso com uma analogia deliciosa e depois falar sobre o **Swagger**, uma ferramenta que ajuda a organizar o "cardápio" das APIs.
## API: O garçom do restaurante digital
Imagine que você está em um restaurante (um aplicativo ou site). Você quer um prato específico, como um **sanduíche de frango com batata frita**. Você não entra na cozinha pra fazer o sanduíche, né? Em vez disso, você fala com o **garçom** (a API). Ele pega seu pedido (uma requisição), leva pra cozinha (o servidor ou sistema), e depois traz o prato pronto (a resposta) pra sua mesa.
**O pedido**: No mundo digital, isso é uma requisição que você envia (ex.: "Quero os dados de um usuário").

**A cozinha**: O servidor que processa o pedido e prepara a "comida" (os dados ou ações).

**O garçom (API)**: A ponte que conecta você ao sistema, sem você precisar saber como a cozinha funciona.

Exemplo prático:
```javascript
fetch('https://api.exemplo.com/sanduiches')
  .then(resposta => resposta.json())
  .then(dados => console.log(dados));
```
Aqui, o fetch é como chamar o garçom pra pedir um sanduíche. A API entrega os dados prontos!
## Por que APIs são importantes?
**Facilidade**: Você não precisa cozinhar (programar tudo do zero).

**Reuso**: Vários clientes (apps, sites) podem pedir o mesmo prato pelo garçom.

**Segurança**: Você não entra na cozinha (não acessa o sistema diretamente).

## Swagger: O cardápio do restaurante
Agora entra o **Swagger**: ele é como o **cardápio** do restaurante. Ele descreve tudo que a API (o garçom) pode oferecer: quais pratos (endpoints) estão disponíveis, como pedir (métodos como GET ou POST), e o que você vai receber (respostas). Com o Swagger, você não precisa adivinhar o que pode pedir — tá tudo escrito e organizado!
Exemplo de um "item do cardápio" no Swagger:
```yaml
/pedir-sanduiche:
  get:
    description: Pega a lista de sanduíches disponíveis
    responses:
      200:
        description: Lista de sanduíches retornada com sucesso
```
Isso diz que você pode "pedir" sanduíches com um GET e vai receber uma lista gostosa de volta.
## Pra que serve o Swagger?
**Documentação**: Mostra o que a API faz, como um cardápio bem feito.

**Testes**: Você pode "provar" o prato direto no Swagger UI, sem cozinhar nada.

**Padronização**: Todo mundo entende o cardápio da mesma forma.

# OpenAPI, Swagger Editor, Swagger UI e Swagger Codegen
Essas ferramentas são essenciais pra criar e gerenciar APIs de forma prática. Vamos ver o que cada uma faz, com explicações simples e um toque leve de analogia pra ajudar a entender.
## OpenAPI: O padrão das APIs
O **OpenAPI** é um formato (YAML ou JSON) que define como uma API funciona: endpoints, métodos (GET, POST, etc.), parâmetros e respostas. É como um "cardápio" que explica o que a API oferece e como pedir.
Exemplo básico:
```yaml
openapi: 3.0.0
info:
  title: API Simples
  version: 1.0.0
paths:
  /usuarios:
    get:
      responses:
        '200':
          description: Lista de usuários retornada
```
Isso descreve um endpoint /usuarios que devolve uma lista com um GET.
## Swagger Editor: Escrevendo a documentação
O **Swagger Editor** é onde você escreve e testa especificações OpenAPI. Pense nele como um caderno onde você anota o "cardápio" da API, ajusta os detalhes e verifica se tá tudo certo. Disponível online (editor.swagger.io) ou localmente.
Exemplo: Cole o YAML acima e veja se tem erros.

Ótimo pra planejar antes de codificar.

## Swagger UI: Visualizando e testando
O **Swagger UI** pega sua especificação OpenAPI e cria uma interface interativa. É como mostrar o cardápio pros outros: você vê os endpoints e pode testar direto no navegador.
No exemplo acima, aparece um botão "GET /usuarios" pra clicar e testar.

Ideal pra compartilhar com outros desenvolvedores.

## Swagger Codegen: Gerando código
O **Swagger Codegen** lê a especificação OpenAPI e gera código pra você, como um ajudante que prepara tudo rapidinho. Ele cria:
**Clientes**: Pra usar a API (ex.: JavaScript, Python).

**Servidores**: Pra implementar a API (ex.: Node.js).

Exemplo: swagger-codegen generate -i api.yaml -l javascript -o ./cliente gera um cliente JS.

## Como elas se conectam?
**OpenAPI**: Define a estrutura.

**Swagger Editor**: Você escreve e valida.

**Swagger UI**: Mostra e testa.

**Swagger Codegen**: Gera o código.
Juntas, elas tornam o trabalho com APIs mais rápido e organizado.

# Autenticidade e Segurança em APIs: Teoria e Prática
Quando você publica uma API importante pro público, garantir **autenticidade** (quem está usando?) e **segurança** (proteger os dados e o sistema) é essencial. Pense nisso como trancar a porta de casa e só deixar entrar quem tem a chave certa. Aqui vai o que você precisa saber na teoria e como aplicar na prática.
## Teoria: Conceitos principais
### Autenticidade
Autenticidade é verificar quem está fazendo a requisição. Sem isso, qualquer um poderia acessar sua API e causar problemas.
**Chaves de API**: Uma "senha" simples que identifica o usuário ou app. Ex.: api_key=12345.

**Tokens (OAuth, JWT)**: Mais avançado, como um "crachá" que prova identidade e permissões. JWT (JSON Web Token) é comum em APIs modernas.

**Por que importa?**: Evita uso não autorizado e rastreia quem usa sua API.

### Segurança
Segurança protege os dados e o servidor contra ataques. Os principais pontos são:
**HTTPS**: Criptografa a comunicação, como selar uma carta antes de enviar.

**Validação de entrada**: Checa se os dados recebidos fazem sentido, pra evitar "lixo" ou ataques como SQL Injection.

**Rate Limiting**: Limita quantas requisições alguém pode fazer, pra não sobrecarregar o sistema.

**CORS**: Controla quais sites podem chamar sua API, evitando uso indevido.

## Prática: Como implementar
### 1. Usando HTTPS
**Teoria**: Sem HTTPS, os dados vão "abertos" e podem ser interceptados.

**Prática**: Use um certificado SSL/TLS (ex.: via Let’s Encrypt). No servidor, configure pra só aceitar https://sua-api.com.

### 2. Chaves de API
**Teoria**: Identifica quem usa a API.

**Prática**: Gere uma chave única pra cada usuário e valide no backend.
```javascript
// Exemplo em Node.js
app.get('/dados', (req, res) => {
const apiKey = req.headers['x-api-key'];
if (apiKey !== 'minha-chave-secreta') {
  return res.status(401).send('Acesso negado');
}
res.send('Dados importantes');
});
```

### 3. Tokens JWT
**Teoria**: Mais seguro que chaves simples, inclui informações assinadas.

**Prática**: Use uma biblioteca como jsonwebtoken em Node.js.
```javascript
const jwt = require('jsonwebtoken');

// Gerar token
const token = jwt.sign({ userId: 1 }, 'segredo', { expiresIn: '1h' });
// Validar token
app.get('/protegido', (req, res) => {
  const token = req.headers['authorization'];
  try {
    const decoded = jwt.verify(token, 'segredo');
    res.send('Acesso liberado');
  } catch (erro) {
    res.status(401).send('Token inválido');
  }
});
```
### 4. Validação de entrada
**Teoria**: Evita dados maliciosos.

**Prática**: Cheque tudo que chega.
```javascript
app.post('/usuario', (req, res) => {
const { nome } = req.body;
if (!nome || typeof nome !== 'string' || nome.length > 50) {
  return res.status(400).send('Nome inválido');
}
res.send('Usuário criado');
});
```

### 5. Rate Limiting
**Teoria**: Protege contra abuso.

**Prática**: Use middleware como express-rate-limit.
```javascript
const rateLimit = require('express-rate-limit');
app.use(rateLimit({
windowMs: 15 * 60 * 1000, // 15 minutos
max: 100 // 100 requisições por IP
}));
```

### 6. CORS
**Teoria**: Limita quem pode chamar a API.

**Prática**: Configure no servidor.
```javascript
const cors = require('cors');
app.use(cors({ origin: 'https://meu-site.com' }));
```

## Dicas pra publicar uma API pública
**Documente bem**: Use OpenAPI/Swagger pra explicar como autenticar e usar.

**Monitore**: Registre requisições pra identificar abusos (ex.: com logs).

**Teste ataques**: Tente quebrar sua API (ethical hacking) pra achar falhas.

**Atualize**: Mantenha bibliotecas e certificados em dia.

## Parte apenas para meu próprio estudo
```yaml
openapi: 3.0.1
info: 
  title: API de consultório
  description: API para controlar médicos e suas especialidades dentro do consultório.
  version: 0.0.1
  termsOfService: https://mockapi.io
  contact:
    name: Suporte a Devs
    email: contato@example.com
    url: https://mockapi.io
  license:
    name: "Lincença: GPLv3"
    url: https://www.gnu.org/licenses/gpl-3.0.html
externalDocs:
  description: Documentação burocrática
  url: https://mockapi.io
servers: # Colocar a API Aqui e de preferencia a parte de segurança
- url: https://67eec20cc11d5ff4bf7ad3d8.mockapi.io/ 
  description: API de Teste
security: 
- auth: []
paths:
  /Especialidades:
    get:
      summary: Recupera todas as especialidades
      tags: # Tags servem para separar além de Default no Swagger UI, como se fossem partes
        - Especialidades
      responses:
        200:
          description: Sucesso
          content: 
            application/json:
              schema:
                  $ref: "#/components/schemas/Specialties"
    post: # Primeira parte da usabilidade da API, onde (post)amos o ID, seria o C de CRUD 
      summary: Cria uma nova especialidade
      tags:  # Tags servem para separar além de Default no Swagger UI, como se fossem partes
        - Especialidades
      requestBody:
        content:
          application/json:
            schema:
              type: object
              properties:
                descricao:  
                  type: string
      responses:
        201:
          description: "Sucesso"
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Specialty"
  /Especialidades/{id}:
    parameters: 
      - name: id
        in: path
        required: true
        schema: 
          type: integer
    get: # R de CRUD, onde lemos uma Especialidade especifica, ex: api/Especialidade/3 (Há outros tipos, ex query)
      summary: Recupera uma entidade pelo ID
      responses:
        200: 
          description: Sucesso
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Specialty" #ref do component (como se fosse uma variavel)
        404:
          description: Especialidade não encontrada
          content:
            application/json:
              example: "Não encontrado"
    put: # U de CRUD, update, Patch se quiser mudar apenas uma parte em especifica
      summary: Atualiza uma especialidade pelo ID
      requestBody:
        content:
          application/json:
            schema: ## como aqui não mexemos no ID, apenas atualizamos, não usamos o $ref
              type: object
              properties:
                descricao:
                  type: string
      responses:
        200:
          description: Especialidade atualizada com sucesso
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Specialty"
        404:
          description: Especialidade não encontrada
          content:
            application/json:
              example: "Não encontrado"
    delete: # D de CRUD, deletamos o ID especifico, TODO o objeto
      summary: Remova uma especialidade pelo ID
      responses:
        204:
          description: Especialidade removida com sucesso
        404:
          description: Especialidade não encontrada
          content:
            application/json:
              example: "Não encontrado"
components: # Components lembram como forma de declarar variaveis, essa daqui é usadas em varias linhas que tem ID
  schemas:
    Specialty:
      type: object
      properties:
        id:
          type: integer
        descricao:
          type: string
    Specialties: # Aqui declaro a variavel junto com a do Array, usada apenas uma vez
      type: array
      items:
        $ref: "#/components/schemas/Specialty"
  securitySchemes: ## Aqui nós declaramos qual a forma de segurança, depois usamos mais ao topo do editor
    auth:
      type: http
      scheme: bearer
      bearerFormat: JWT
            
            
      ```