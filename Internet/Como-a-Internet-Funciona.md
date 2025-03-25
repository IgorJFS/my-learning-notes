# Como a Internet Funciona

A **internet** é uma rede global que conecta computadores e permite que eles troquem informações. A **web** (World Wide Web) é uma parte dela, baseada em páginas e links que acessamos por navegadores. Aqui está como isso tudo funciona, incluindo os papéis de **DNS**, **TCP/IP**, **HTTP** e **HTTPS**.

## 1. Como a Web Funciona
A **web** é um sistema baseado em navegadores, servidores e conteúdo (HTML, CSS, JS), que depende de protocolos como HTTP/HTTPS.

- **Passo a passo**:
  1. Você digita um **URL** (ex.: "www.exemplo.com") no navegador.
  2. O navegador usa o **DNS** pra traduzir "exemplo.com" num endereço IP (ex.: 192.0.2.1).
  3. O pedido vai pro servidor com esse IP, usando HTTP/HTTPS.
  4. O servidor devolve os arquivos (HTML, imagens, etc.), e o navegador renderiza a página.
- **Exemplo**: Ao acessar "www.youtube.com", o DNS acha o IP, o pedido vai ao servidor, e você vê os vídeos.
- **Curiosidade**: A web é só uma parte da internet — e-mails e apps como WhatsApp usam outros protocolos!

## 2. DNS (Domain Name System)
O **DNS** é como uma "lista telefônica" da internet, traduzindo nomes de domínio em endereços IP.

- **O que é**: Um sistema que converte "www.google.com" num IP (ex.: 142.250.190.78) que os computadores entendem.
- **Como funciona**: O navegador pergunta a um servidor DNS "qual o IP de exemplo.com?", e o DNS responde com o número.
- **Exemplo**: Digitar "www.netflix.com" faz o DNS buscar o IP pra conectar você ao servidor da Netflix.
- **Detalhe**: Sem DNS, teríamos que decorar IPs numéricos pra cada site!

## 3. TCP/IP (Transmission Control Protocol/Internet Protocol)
O **TCP/IP** é a base da comunicação na internet, garantindo que os dados cheguem corretos e na ordem certa.

- **O que é**: Um conjunto de regras. **TCP** divide os dados em pacotes e verifica se chegam bem; **IP** define os endereços e rotas.
- **Como funciona**: Dados (ex.: uma página HTML) são quebrados em pacotes, enviados por IP, e remontados por TCP.
- **Exemplo**: Ao carregar "www.wikipedia.org", o TCP/IP leva os pacotes do servidor até seu computador.
- **Detalhe**: É como o "correio" da internet, entregando pedaços de informação.

## 4. HTTP (HyperText Transfer Protocol)
O **HTTP** é o protocolo que define como os dados são pedidos e enviados entre um cliente (seu navegador) e um servidor.

- **O que é**: Um conjunto de regras pra comunicação. Quando você digita "www.exemplo.com", o navegador usa HTTP pra pedir a página.
- **Como funciona**:
  1. Você faz um **pedido** (request): "Quero a página inicial".
  2. O servidor responde com uma **resposta** (response): "Aqui está o HTML".
- **Exemplo**: Ao entrar em "www.google.com", seu navegador envia um HTTP GET, e o servidor devolve a página de busca.
- **Detalhe**: HTTP não é seguro — os dados viajam "abertos", como uma carta sem envelope.

## 5. HTTPS (HTTP Secure)
O **HTTPS** é a versão segura do HTTP, com criptografia pra proteger os dados.

- **O que é**: Usa **SSL/TLS** pra criptografar a comunicação, impedindo que hackers leiam ou alterem os dados.
- **Como funciona**:
  1. O navegador e o servidor fazem um "aperto de mãos" (handshake) pra criar uma chave secreta.
  2. Tudo que vai e vem é criptografado com essa chave.
- **Exemplo**: Em "https://banco.com", seus dados de login ficam protegidos por HTTPS.
- **Detalhe**: Você vê um cadeado no navegador — sinal de segurança!

## Resumo com Analogia: Pedindo uma Pizza
Pensar na internet e na web como pedir uma pizza ajuda a entender:

- **Web**: O cardápio online (site) é o que você vê, e o motoboy (servidor) entrega a pizza.
- **DNS**: O endereço da pizzaria (ex.: "Rua Exemplo, 123") que o GPS (DNS) traduz pra levar o motoboy até lá.
- **TCP/IP**: A cozinha divide a pizza em fatias (pacotes), e o motoboy as entrega na ordem certa até sua casa.
- **HTTP**: Você liga e pede "uma pizza de calabresa" em voz alta — o pedido vai, mas qualquer um pode ouvir.
- **HTTPS**: Você usa um app seguro pra pedir, e o pedido vai "trancado" num cofre — só a pizzaria abre.