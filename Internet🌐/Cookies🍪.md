## Tudo Sobre Cookies - Os "Bilhetinhos" da Internet
Vamos mergulhar no mundo dos cookies, esses pedacinhos de informação que sites deixam no seu navegador. Eles são como lembretes que ajudam a internet a lembrar de você, e têm um papel importante tanto pro usuário quanto pro desenvolvedor. Abaixo, vamos explorar isso em duas partes: uma mais tranquila pra entender o básico, e outra mais técnica pra quem curte o lado web dev.

Parte 1: Cookies de Forma Simples
Cookies são como bilhetinhos que um site deixa no seu navegador pra lembrar coisas sobre você. Não são arquivos grandes ou programas, só texto simples que guardam informações úteis.

**O que são**: Pequenos dados (nome-valor) que o servidor envia pro seu navegador. Ex.: "Você já logou" ou "Você prefere o site em português".

**Como funciona**:  
Você entra num site (ex.: uma loja online).

O servidor manda um cookie pro seu navegador (ex.: "carrinho=3 itens").

O navegador guarda esse cookie e manda de volta nas próximas visitas pra o site "lembrar" de você.

**Pra que serve**:  
Mantém você logado (não precisa digitar senha toda hora).

Salva preferências (ex.: tema escuro).

Rastreia coisas (ex.: anúncios que te seguem por aí).

**Exemplo real**: Você adiciona um tênis no carrinho de uma loja. O cookie guarda isso. Fecha o navegador, volta depois, e o tênis ainda tá lá!

**Onde ficam**: No seu navegador (Chrome, Firefox), em uma área escondida de armazenamento. Você pode ver ou apagar eles nas configurações.

**Segurança**: Cookies podem ser criptografados (HTTPS), mas não guardam segredos grandes — só referências, como um ID de sessão.

**Analogia**: É como um garçom que anota seu pedido num papelzinho ("mesa 5 quer pizza de calabresa") e guarda pra próxima vez que você voltar ao restaurante. Ele não sabe tudo sobre você, só o que precisa pra te atender bem!

Parte 2: Cookies no Mundo do Web Dev
Agora, vamos pro lado técnico dos cookies, onde eles viram ferramentas poderosas pra desenvolvedores web. Aqui entra a mágica de como a internet funciona sem "memória" e como os cookies ajudam nisso.
**O que são no fundo**: Cookies são pares chave-valor (ex.: `nome=valor`) enviados pelo servidor via cabeçalhos HTTP (`Set-Cookie`) e armazenados pelo cliente. São limitados a 4KB de tamanho por cookie.

**Web é Stateless**:  
A internet (HTTP) não "lembra" de você entre pedidos. Cada clique ou página é uma conversa nova entre cliente e servidor, como se nunca tivessem se falado antes.

Cookies resolvem isso salvando um "estado" no cliente. Ex.: Um cookie com `session_id=abc123` diz ao servidor "ei, sou o mesmo cara de antes".

**Como vira JSON**:  
Cookies em si são texto simples (ex.: `usuario=igor`), mas pra dados complexos, o servidor pode codificar um objeto JSON e mandar como valor. Ex.: `dados={"nome":"igor","idade":25}`.

O navegador não entende JSON direto — ele só guarda o texto. O servidor ou o JavaScript no client side decodifica isso depois (ex.: `JSON.parse()`).

**Cabeçalhos HTTP**:  
**Envio**: O servidor usa `Set-Cookie: chave=valor; expires=Data; path=/` pra definir um cookie. Ex.: `Set-Cookie: tema=escuro; expires=Fri, 31 Dec 2025`.

**Retorno**: O navegador inclui cookies em cada requisição via `Cookie: chave=valor`. Ex.: `Cookie: tema=escuro`.

**Atributos importantes**:  
**Expires/Max-Age**: Define quanto tempo o cookie vive (ex.: 1 ano ou 3600 segundos).

**Path**: Limita onde o cookie vale (ex.: `/blog` só pro blog).

**HttpOnly**: Bloqueia acesso via JavaScript, mais seguro contra ataques.

**Secure**: Só via HTTPS, pra proteger os dados.

**Sessões**:  
Cookies muitas vezes guardam um ID de sessão (ex.: `session_id=xyz789`), que o servidor usa pra puxar infos de um banco de dados (ex.: seu perfil).

Exemplo: Você loga, o servidor cria uma sessão no banco (`xyz789: {usuario: igor}`) e manda o cookie pro cliente.

**Exemplo técnico**:  
Requisição: `GET /perfil HTTP/1.1` com `Cookie: session_id=xyz789`.

Resposta: O servidor vê `xyz789`, busca no banco e devolve seu perfil em HTML ou JSON.

**Analogia**: Cookies são como um crachá de funcionário. O site (servidor) te dá um crachá com "ID: 123" (cookie). A web não lembra quem você é (stateless), mas toda vez que você volta, mostra o crachá, e o servidor diz: "Ah, é o 123, sei quem você é!" O JSON é tipo um crachá com mais detalhes (nome, cargo), mas o porteiro (servidor) decifra.

Site da Alura com bastante informações sobre os cookies! [https://www.alura.com.br/artigos/o-que-sao-cookies-como-funcionam](https://www.alura.com.br/artigos/o-que-sao-cookies-como-funcionam)