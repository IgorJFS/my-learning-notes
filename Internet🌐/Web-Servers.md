# Nginx, Apache, Cloudflare e WebSockets
**Objetivo:** Entender o que é Nginx, Apache, Cloudflare e ter uma visão básica sobre WebSockets.  
O que é Nginx?
**Nginx** (pronuncia-se "engine-x") é um servidor web e proxy reverso leve e de alta performance.  

Criado pra lidar com muitas conexões simultâneas, é ótimo pra sites grandes ou APIs.  

Usos:  
Servir páginas estáticas (HTML, CSS, JS).  

Proxy reverso pra encaminhar requisições a outros servidores.  

Balanceamento de carga.

Exemplo: `proxy_pass http://localhost:3000;\` roteia tráfego pra um backend.

O que é Apache?
**Apache HTTP Server** é um servidor web de código aberto, mais antigo e muito usado.  

Focado em flexibilidade, suporta muitos módulos (ex.: PHP, SSL).  

Usos:  
Hospedar sites dinâmicos (com `.htaccess` pra regras).  

Servir conteúdo estático ou scripts (ex.: WordPress).

Diferença pro Nginx: Apache é mais "pesado" mas mais configurável por diretório.

O que é Cloudflare?
**Cloudflare** é uma empresa/solução de infraestrutura web que oferece CDN, segurança e otimização.  

Funciona como um "intermediário" entre o cliente e seu servidor.  

Recursos:  
**CDN:** Armazena cópias do site em servidores globais pra carregar mais rápido.  

**DDoS Protection:** Bloqueia ataques de tráfego malicioso.  

**DNS:** Gerencia nomes de domínio com opções como DNSSEC.

Exemplo: Seu site `meudominio.com` usa Cloudflare pra acelerar e proteger.

O que são WebSockets?
**WebSockets** é um protocolo pra comunicação bidirecional em tempo real entre cliente (ex.: navegador) e servidor.  

Diferente do HTTP (requisição-resposta), mantém uma conexão aberta.  

Usos:  
Chats ao vivo.  

Jogos online.  

Atualizações em tempo real (ex.: preços de ações).

Como funciona: Após um "handshake" HTTP, a conexão vira um canal persistente.  

Exemplo no Nginx:
```nginx
location /ws {
proxy_pass http://localhost:8080;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
}
```

Notas de Aprendizado
Nginx é leve e rápido, Apache é flexível e robusto.  

Cloudflare adiciona segurança e velocidade sem mudar seu servidor.  

WebSockets são perfeitos pra interatividade em tempo real!

