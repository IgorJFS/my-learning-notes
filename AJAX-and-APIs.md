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

## Axios  
**Axios** é uma biblioteca simples para requisições HTTP, com suporte a Promises.

- **Exemplo**:  
  ```javascript  
  axios.get("https://api.exemplo.com/dados")  
    .then(res => console.log(res.data));  
  ```  
- **Por que usar**: Configuração fácil, tratamento automático de JSON.
