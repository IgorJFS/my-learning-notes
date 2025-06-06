# CORS (Cross-Origin Resource Sharing)

O CORS (Compartilhamento de Recursos entre Origens Diferentes) é um mecanismo de segurança implementado pelos navegadores para controlar como recursos de uma página web podem ser requisitados a partir de outro domínio (origem).

---

## 1. O que é Origem?

- Uma origem é definida pelo protocolo, domínio e porta. Exemplo:
  - `https://exemplo.com:443` é diferente de `http://exemplo.com:80`.

---

## 2. Por que o CORS existe?

- Para proteger os usuários contra ataques como Cross-Site Request Forgery (CSRF) e vazamento de dados.
- Por padrão, requisições AJAX/fetch só podem acessar recursos da mesma origem.

---

## 3. Como funciona o CORS?

- O servidor deve enviar cabeçalhos HTTP específicos permitindo ou negando o acesso de outras origens.
- Principais cabeçalhos:
  - `Access-Control-Allow-Origin`: Especifica quais origens podem acessar o recurso.
  - `Access-Control-Allow-Methods`: Métodos HTTP permitidos (GET, POST, etc).
  - `Access-Control-Allow-Headers`: Quais cabeçalhos personalizados podem ser usados.
  - `Access-Control-Allow-Credentials`: Permite envio de cookies/autenticação.

---

## 4. Tipos de Requisições

- **Simple Requests**: GET, POST (com certos tipos de conteúdo), HEAD.
- **Preflight Requests**: Para métodos/cabeçalhos não simples, o navegador envia uma requisição OPTIONS antes da principal para checar permissões.

---

## 5. Exemplo de Resposta CORS

```
Access-Control-Allow-Origin: https://meusite.com
Access-Control-Allow-Methods: GET, POST
Access-Control-Allow-Headers: Content-Type
```

---

## 6. Erros Comuns

- Falta de cabeçalhos CORS no servidor.
- Tentar usar credenciais sem `Access-Control-Allow-Credentials: true`.
- Origem não permitida.

---

## 7. Referências

- [MDN Web Docs - CORS](https://developer.mozilla.org/pt-BR/docs/Web/HTTP/CORS)
- [OWASP - CORS](https://owasp.org/www-community/attacks/CORS_OriginHeaderScrutiny)
- [Video do Dias de Dev](https://www.youtube.com/watch?v=Fha6Il-5RYE&t=659s)
