# Guia Completo sobre Docker

## Objetivo

Entender o que é Docker, por que ele é importante, como funciona, e como usá-lo em projetos reais, com exemplos práticos e boas práticas. Este guia vai do básico ao avançado, com analogias pra facilitar e instruções pra integrar banco de dados, backend e frontend usando Docker Compose.

## Introdução: O que é Docker?

Docker é uma ferramenta que permite *empacotar* aplicativos e suas dependências (bibliotecas, configurações, etc.) em *contêineres*. Pense nos contêineres como caixas de delivery de comida: cada caixa tem tudo que o prato precisa (ingredientes, temperos, instruções) pra funcionar igualzinho em qualquer lugar, seja na sua casa ou na casa do vizinho. No caso do Docker, essas caixas rodam seu aplicativo da mesma forma em qualquer computador, servidor ou nuvem.

### O que Docker foi feito pra resolver?

Antes do Docker, era comum ter problemas tipo "funciona na minha máquina, mas não no servidor". Isso acontecia por diferenças nos ambientes: versões de bibliotecas, sistemas operacionais, configurações. Docker resolve isso criando contêineres que são *portáteis* e *consistentes*. Ele também facilita:

- **Escalabilidade**: Rodar várias caixas (contêineres) em servidores diferentes.
- **Isolamento**: Cada contêiner é independente, como se fosse uma máquina virtual, mas mais leve.
- **Desenvolvimento**: Testar apps em ambientes idênticos ao de produção.

### Por que Docker é importante hoje?

Hoje, com microsserviços e computação em nuvem, Docker é quase onipresente. Empresas como Netflix, Spotify e Uber o usam pra rodar milhares de contêineres. Ele é essencial pra:

- **CI/CD**: Automatizar deploy com ferramentas como Jenkins ou GitHub Actions.
- **Nuvem**: Integra com AWS, Azure, Google Cloud.
- **DevOps**: Desenvolvedores e ops trabalham com o mesmo ambiente.
- **Produtividade**: Configurar um projeto é tão simples quanto "docker run".

## Como o Docker funciona? (Técnico, mas com analogia)

Docker usa *contêineres*, que são como lanches prontos: cada um tem os ingredientes (código, bibliotecas, configs) e é preparado com base numa receita (uma imagem). A imagem é um *molde* imutável, e os contêineres são as cópias que rodam.

### Visual do Docker (ASCII)

Imagine o Docker como uma cozinha:

```bash
+-------------------+
| Sistema Operacional (ex.: Linux) |
+-------------------+
| Docker Engine (o chef) |
+-------------------+
| Imagem (receita) |
| - Nginx |
| - MySQL |
| - Node.js |
+-------------------+
| Contêineres (pratos prontos) |
| - Contêiner 1: Nginx |
| - Contêiner 2: MySQL |
| - Contêiner 3: Node.js |
+-------------------+
```
- **Docker Engine**: O "chef" que gerencia as imagens e contêineres.
- **Imagem**: A receita estática (ex.: "Ubuntu + Node.js v18").
- **Contêiner**: O prato pronto, rodando a partir da imagem.

### Diferença entre contêiner e máquina virtual

Contêineres são leves porque *compartilham* o kernel do sistema operacional. Máquinas virtuais (VMs) precisam de um SO completo, o que as torna mais pesadas.

Contêiner: Só o app + bibliotecas (1 MB a 1 GB)
VM: SO inteiro + app (vários GBs)

## Sintaxe básica do Docker

Aqui vão os comandos principais pra usar Docker. Vamos supor que você quer rodar um servidor Nginx.

1. **Puxar uma imagem** (do Docker Hub, tipo um mercado de receitas):
   ```bash
   docker pull nginx:latest

**Rodar um contêiner**:
```bash

docker run -d -p 8080:80 nginx
```
-d: Roda em background.

-p 8080:80: Mapeia a porta 8080 do host pra porta 80 do contêiner.

**Listar contêineres rodando**:
```bash

docker ps
```
**Parar um contêiner**:
```bash

docker stop <container_id>
```
**Ver imagens disponíveis**:
```bash

docker images
```
### Exemplo prático: Rodando um app Node.js
Crie um arquivo app.js:
```javascript

const express = require('express');
const app = express();
app.get('/', (req, res) => res.send('Olá, Docker!'));
app.listen(3000, () => console.log('Rodando na porta 3000'));
```
Agora, crie um Dockerfile pra empacotar esse app.

## O que é Dockerfile?
Um Dockerfile é como uma receita escrita: ele diz ao Docker como construir uma imagem. Cada linha é uma instrução.
Exemplo de `Dockerfile` pro app Node.js:
```dockerfile
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["node", "app.js"]
```
#### Explicando cada linha (com analogia de receita)

```
**FROM node:18**: "Use a base de bolo Node.js versão 18" (começa com uma imagem base).

**WORKDIR /app**: "Prepare a mesa na pasta /app" (define o diretório de trabalho).

**COPY package*.json ./**: "Pegue a lista de ingredientes (package.json)" (copia arquivos).

**RUN npm install**: "Misture os ingredientes" (instala dependências).

**COPY . .**: "Adicione o resto do bolo (código)" (copia tudo).

**EXPOSE 3000**: "Diga que o prato sai na porta 3000" (indica a porta).

**CMD ["node", "app.js"]**: "Asse o bolo!" (comando pra rodar o app).
```
### Construindo e rodando
Construa a imagem:
```bash

docker build -t meu-app .
```
Rode o contêiner:
```bash

docker run -d -p 3000:3000 meu-app
```
Acesse `http://localhost:3000` e veja "Olá, Docker!".

## O que é Docker Compose?
Docker Compose é como um livro de receitas pra rodar vários contêineres juntos. Ele usa um arquivo YAML (`docker-compose.yml`) pra definir serviços, redes e volumes.

### Analogia boba
Pense no Docker Compose como um jantar com entrada, prato principal e sobremesa. Cada prato é um contêiner (ex.: banco de dados, backend, frontend), e o Compose é o garçom que coordena tudo pra sair na ordem certa.

## Exemplo: Banco de dados + Backend + Frontend
Vamos criar um projeto com:
**MySQL**: Banco de dados.

**Node.js**: Backend com Express.

**React**: Frontend.

Crie um arquivo `docker-compose.yml`:
```
version: '3.8'
services:
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: senha123
      MYSQL_DATABASE: loja
    volumes:
      - db-data:/var/lib/mysql
    networks:
      - app-network
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    depends_on:
      - db
    networks:
      - app-network
  frontend:
    build:
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "80:80"
    depends_on:
      - backend
    networks:
      - app-network
networks:
  app-network:
    driver: bridge
volumes:
  db-data:
```

### Explicando o docker-compose.yml

**version**: Versão do Compose (3.8 é comum).

**services**: Lista de contêineres (db, backend, frontend).

**image**: Usa uma imagem pronta (ex.: mysql:8.0).

**build**: Constrói uma imagem a partir de um Dockerfile.

**environment**: Configura variáveis (ex.: senha do MySQL).

**ports**: Mapeia portas (ex.: 3000 do backend pro host).

**depends_on**: Define ordem de inicialização (ex.: backend espera db).

**networks**: Cria uma rede pra os contêineres se comunicarem.

**volumes**: Persiste dados (ex.: dados do MySQL).

### Estrutura do projeto
```
projeto/
├── backend/
│   ├── Dockerfile
│   ├── app.js
│   ├── package.json
├── frontend/
│   ├── Dockerfile
│   ├── src/
├── docker-compose.yml
```
#### Rodando tudo
```bash

docker-compose up -d
```
Acesse:
Frontend: `http://localhost`

Backend: `http://localhost:3000`

MySQL: Conecte via `mysql -h localhost -u root -p`.

## Docker Hub (Docker Cloud)
Docker Hub é o "mercado de receitas" do Docker. Lá você encontra imagens prontas (ex.: `nginx, mysql, node`) e pode publicar suas próprias imagens.
Como usar
Crie uma conta em hub.docker.com

Faça login:
```bash

docker login
```
Publique uma imagem:
```bash

docker tag meu-app seu-usuario/meu-app:1.0
docker push seu-usuario/meu-app:1.0
```
Puxe sua imagem em outro lugar:
```bash

docker pull seu-usuario/meu-app:1.0
```
Docker Hub também tem planos pagos pra repositórios privados e integração com CI/CD.
Boas práticas com Docker
**Use imagens leves**: Prefira node:18-alpine (pequena) em vez de node:18 (pesada).

**Evite rodar tudo como root**: Adicione um usuário no Dockerfile:
```
RUN useradd -m appuser
USER appuser
```
**Limpe imagens e contêineres antigos**:
```bash

docker system prune
```
**Use .dockerignore**: Evite copiar arquivos desnecessários (ex.: node_modules).
```
node_modules
.git
```
**Defina recursos**: Limite CPU e memória no docker-compose.yml:
```
deploy:
  resources:
    limits:
      cpus: '0.5'
      memory: 512M
```
**Versione imagens**: Use tags específicas (ex.: mysql:8.0 em vez de mysql:latest).

**Teste localmente**: Sempre rode docker-compose up antes de deploy.

## Quando usar Docker?
Docker faz sentido em:
**Projetos com várias dependências**: Ex.: app com banco, cache, e API.

**Ambientes consistentes**: Pra evitar "funciona na minha máquina".

**Microsserviços**: Cada serviço em um contêiner.

**CI/CD**: Testes e deploys automatizados.

**Nuvem**: Integra com Kubernetes, AWS ECS, etc.

Não use Docker se:
O projeto é muito simples (ex.: um script Python sem dependências).

Você não precisa de isolamento ou portabilidade.
