# Nginx, Apache, Cloudflare e WebSockets
**Objetivo:** Entender o que é Nginx, Apache, Cloudflare e ter uma visão básica sobre WebSockets.  
## O que é Nginx?
**Nginx** (pronuncia-se "engine-x") é um servidor web e proxy reverso leve e de alta performance.  

Criado pra lidar com muitas conexões simultâneas, é ótimo pra sites grandes ou APIs.  

Usos:  
Servir páginas estáticas (HTML, CSS, JS).  

Proxy reverso pra encaminhar requisições a outros servidores.  

Balanceamento de carga com algoritmos como Round Robin e Least Connections.

## Conceitos de Balanceamento no Nginx
**Round Robin:**  
Método padrão de balanceamento. Distribui requisições sequencialmente entre os servidores disponíveis.  

Exemplo: Se tem 3 servidores, a 1ª requisição vai pro Server1, a 2ª pro Server2, a 3ª pro Server3, e repete.  

Bom pra cargas uniformes, mas não considera o "peso" dos servidores.

**Least Connections:**  
Direciona requisições pro servidor com menos conexões ativas no momento.  

Ideal pra quando os servidores têm capacidades diferentes ou cargas variáveis.  

Exemplo: Se Server1 tá com 5 conexões e Server2 com 2, a próxima vai pro Server2.

**Backup:**  
Define servidores reserva que só entram em ação se os principais falharem.  

Útil pra alta disponibilidade: se o Server1 cai, o Backup assume.

## Exemplo de Configuração no Nginx
Aqui tá um exemplo completo com Round Robin, Least Connections e Backup:
```nginx
http {
    # Define um grupo de servidores pra balanceamento
    upstream backend {
        # Least Connections (substitui Round Robin padrão)
        least_conn;  

    \# Servidores principais  
    server localhost:3000;  \# Backend 1  
    server localhost:3001;  \# Backend 2  

    \# Servidor reserva  
    server localhost:3002 backup;  \# Só entra se os outros falharem  
\}  

server \{  
    listen 80;  
    server_name api.meudominio.com;  

    \# Proxy reverso com balanceamento  
    location / \{  
        proxy_pass http://backend;  
        proxy_set_header Host \$host;  
        proxy_set_header X-Real-IP \$remote_addr;  
    \}  

    \# WebSocket (exemplo extra)  
    location /ws \{  
        proxy_pass http://backend;  
        proxy_http_version 1.1;  
        proxy_set_header Upgrade \$http_upgrade;  
        proxy_set_header Connection "upgrade";  
    \}  

    \# Página de erro personalizada  
    error_page 404 /erro.html;  
    location = /erro.html \{  
        root C:/Tools/nginx-1.27.4/html;  
        internal;  
    \}  
\}  

}
```  
**upstream:** Define o grupo de servidores pra balancear.  

**least_conn:** Usa Least Connections (pra Round Robin, é só tirar essa linha).  

**backup:** Marca o servidor reserva.  

**proxy_pass:** Encaminha pro grupo `backend`.

Testando
Salve em `C:/Tools/nginx-1.27.4/conf/nginx.conf`.  

Teste: `nginx.exe -t`.  

Recarregue: `nginx.exe -s reload`.  

Acesse `http://localhost/\` e veja o balanceamento em ação!

Para uso de site real: 
```nginx
server {
    listen 443 ssl;
    server_name meusite.com; # Substitua por seu domínio real

    root C:/Users/User/Desktop/perfomance;
    index index.html;

    # Certificados Let’s Encrypt
    ssl_certificate C:/ProgramData/letsencrypt/live/meusite.com/fullchain.pem;
    ssl_certificate_key C:/ProgramData/letsencrypt/live/meusite.com/privkey.pem;

    # Segurança extra
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;

    gzip on;
    gzip_types text/css;

    add_header Keep-Alive "timeout=5, max=1000";

    location / {
        try_files $uri $uri/ /index.html;
        proxy_cache my_cache;
        proxy_cache_valid 200 30m;
        add_header X-Cache-Status $upstream_cache_status;
    }
    
    location ~ \.png$ {
        expires 30d;
        add_header Cache-Control "public";
        proxy_cache my_cache;
        proxy_cache_valid 200 60m;
    }
}

server {
    listen 80; # Redireciona HTTP pra HTTPS
    server_name meusite.com;
    return 301 https://$host$request_uri;
}

server {
    listen 8005; # Mantém o teste sem SSL
    server_name localhost;

    root C:/Users/User/Desktop/perfomance;
    index index.html;

    gzip on;
    gzip_types text/css;

    add_header Keep-Alive "timeout=5, max=1000";

    location / {
        try_files $uri $uri/ /index.html;
        proxy_cache my_cache;
        proxy_cache_valid 200 30m;
        add_header X-Cache-Status $upstream_cache_status;
    }
    
    location ~ \.png$ {
        expires 30d;
        add_header Cache-Control "public";
        proxy_cache my_cache;
        proxy_cache_valid 200 60m;
    }
}
```

## Apache
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

## O que são WebSockets?
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

## Notas de Aprendizado
Nginx é leve e rápido, Apache é flexível e robusto.  

Cloudflare adiciona segurança e velocidade sem mudar seu servidor.  

WebSockets são perfeitos pra interatividade em tempo real!

