# AJAX e APIs

Este documento explora os fundamentos de AJAX e APIs, ferramentas essenciais para comunicação com servidores na web.

## AJAX  
**AJAX** (Asynchronous JavaScript and XML) permite atualizar partes de uma página sem recarregá-la inteira, usando JavaScript assíncrono.

- **O que é**: Técnica para fazer requisições ao servidor em segundo plano.  
- **Por que usar**: Melhora a experiência do usuário (ex.: carregar posts sem refresh).  
- **Exemplo básico**: Usa `XMLHttpRequest` ou `fetch` (ver abaixo).

## APIs  
**APIs** (Application Programming Interfaces) são conjuntos de regras que permitem a comunicação entre sistemas, como cliente e servidor.

- **O que é**: Uma "ponte" para acessar dados ou serviços (ex.: API do Twitter).  
- **Por que usar**: Conecta seu código a funcionalidades externas.  
- **Exemplo**: API REST retorna dados em JSON.

## JSON  
**JSON** (JavaScript Object Notation) é um formato leve para trocar dados, fácil de ler e escrever.

- **`JSON.stringify`**: Converte um objeto JS em string JSON.  
- **`JSON.parse`**: Converte uma string JSON em objeto JS.  
- **Exemplo**:  
  ```javascript  
  const obj = { nome: "Ana" };  
  const json = JSON.stringify(obj); // '{"nome":"Ana"}'  
  const back = JSON.parse(json);    // { nome: "Ana" }  
  ```

## Hoppscotch (Postman)  
**Hoppscotch** é uma ferramenta para testar APIs, similar ao Postman, mas leve e online.

- **O que faz**: Envia requisições e exibe respostas (ex.: GET, POST).  
- **Por que usar**: Testa endpoints antes de codificar.

## HTTP Verbs  
**HTTP Verbs** definem ações em requisições:

- **GET**: Pega dados.  
- **POST**: Envia dados.  
- **PUT**: Atualiza dados.  
- **DELETE**: Remove dados.

## HTTP Status Codes  
**Códigos de Status HTTP** indicam o resultado de uma requisição:

- **200**: OK, sucesso.  
- **404**: Não encontrado.  
- **500**: Erro no servidor.

## Query Strings  
**Query Strings** são parâmetros na URL para enviar dados ao servidor.

- **Formato**: `?chave=valor&outra=valor`.  
- **Exemplo**: `site.com/users?id=1` (busca usuário com ID 1).

## HTTP Headers  
**Headers** adicionam metadados às requisições/respostas.

- **Exemplos**:  
  - `Content-Type: application/json`: Tipo de dado enviado.  
  - `Authorization`: Token de autenticação.

## Making XHRs  
**XMLHttpRequest (XHR)** é o método clássico para requisições AJAX.

- **Exemplo**:  
  ```javascript  
  const xhr = new XMLHttpRequest();  
  xhr.open("GET", "https://api.exemplo.com/dados");  
  xhr.onload = () => console.log(JSON.parse(xhr.response));  
  xhr.send();  
  ```

## Fetch API  
**Fetch API** é uma alternativa moderna ao XHR, baseada em Promises, para fazer requisições HTTP de forma simples.

- **O que faz**: Envia requisições ao servidor e retorna uma Promise com a resposta.  
- **Por que usar**: Mais limpo que XHR, integrado ao JS nativo.  
- **Exemplo GET**:  
  ```javascript  
  fetch("https://api.exemplo.com/dados")  
    .then(res => res.json())  
    .then(data => console.log(data));  
  ```  
- **Exemplo POST**: Envia dados ao servidor (ex.: criar um usuário).  
  ```javascript  
  fetch("https://api.exemplo.com/usuarios", {  
    method: "POST",  
    headers: { "Content-Type": "application/json" },  
    body: JSON.stringify({ nome: "João", idade: 25 })  
  })  
    .then(res => res.json())  
    .then(data => console.log("Usuário criado:", data));  
  ```

  ## Async/Await com Fetch  
**Async/Await** combina com `fetch` para escrever requisições de forma mais clara, como se fossem síncronas, mas sem bloquear o código.

- **O que faz**: Usa `async` para marcar a função como assíncrona e `await` para esperar a resposta da Promise do `fetch`.  
- **Por que usar**: Evita encadeamentos de `.then()`, tornando o código mais legível e fácil de tratar erros.  
- **Como funciona**:  
  - `fetch` retorna uma Promise com a resposta.  
  - Primeiro `await` pega a resposta bruta (`Response`).  
  - Segundo `await` (com `.json()`) converte para dados utilizáveis.  
- **Exemplo**:  
  ```javascript  
  async function getUser(id) {  
    try {  
      const response = await fetch(`https://api.exemplo.com/usuarios/${id}`);  
      if (!response.ok) throw new Error(`Erro ${response.status}`);  
      const data = await response.json();  
      console.log("Usuário:", data);  
    } catch (error) {  
      console.log("Falha:", error.message);  
    }  
  }  
  getUser(1);  
  ```  
- **Detalhe**: O `try/catch` captura erros como falhas na rede ou status HTTP ruins (ex.: 404).

## Axios: Requisições HTTP Simples com Promises
Axios é uma biblioteca leve e popular para fazer requisições HTTP em JavaScript, com suporte nativo a **Promises** e uma API simples. Funciona tanto no navegador quanto no Node.js, sendo uma alternativa ao `fetch` nativo, mas com vantagens como tratamento automático de JSON e configuração mais intuitiva.
**Por que usar Axios?**
-**Simplicidade**: Sintaxe clara e menos código boilerplate que `fetch`.

-**JSON automático**: Converte respostas JSON sem precisar de `.json()`.

-**Configuração fácil**: Permite definir cabeçalhos, timeouts, etc., de forma direta.

-**Interceptadores**: Útil para adicionar regras globais (ex.: tokens de autenticação).

-**Erros**: Trata erros HTTP (ex.: 404, 500) de forma mais amigável.

-**Exemplo Básico**:
```javascript
axios.get("https://api.exemplo.com/dados")
  .then(res => console.log(res.data))
  .catch(err => console.error("Erro:", err));
```
`res.data`: Contém os dados retornados pela API.

`.catch`: Captura erros de rede ou da requisição.

-**Exemplo com Configuração**:
```javascript
axios({
  method: "post",
  url: "https://api.exemplo.com/criar",
  data: { nome: "Zackie", idade: 25 },
  headers: { "Content-Type": "application/json" }
})
  .then(res => console.log("Sucesso:", res.data))
  .catch(err => console.error("Falha:", err.response));
```
`data`: Envia um corpo na requisição.

`err.response`: Detalha o erro (status, dados, etc.).

**Dica Prática**:
Use `async/await` para um código mais limpo:
```javascript
async function fetchDados() {
  try {
    const res = await axios.get("https://api.exemplo.com/dados");
    console.log(res.data);
  } catch (err) {
    console.error("Erro:", err);
  }
}
fetchDados();
```


