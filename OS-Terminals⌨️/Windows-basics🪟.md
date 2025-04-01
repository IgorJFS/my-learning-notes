# CMD Basics
**Objetivo:** Aprender comandos básicos do CMD (Prompt de Comando do Windows) úteis para um web dev iniciante.  
**O que é?**
O CMD é o terminal padrão do Windows. Apesar de o Linux ser mais comum em servidores, saber CMD ajuda em tarefas locais, como gerenciar arquivos ou rodar scripts.  
**Comandos básicos:**  
**DIR**  
Lista arquivos e pastas no diretório atual.  

Exemplo:
```cmd
DIR
```  

No Linux: `ls` (ou `dir` em algumas distros).

**CD**  
Navega entre diretórios.  

Exemplo:
```cmd
CD pasta-projeto
```  

No Linux: `cd pasta-projeto`.

**MKDIR**  
Cria uma nova pasta.  

Exemplo:
```cmd
MKDIR meu-site
```  

No Linux: `mkdir meu-site`.

**DEL**  
Deleta arquivos.  

Exemplo:
```cmd
DEL arquivo.txt
```  

No Linux: `rm arquivo.txt`.

**MOVE**  
Move ou renomeia arquivos/pastas.  

Exemplo:
```cmd
MOVE velho-nome.js novo-nome.js
```  

No Linux: `mv velho-nome.js novo-nome.js`.

**CLS**  
Limpa a tela do terminal.  

Exemplo:
```cmd
CLS
```  

No Linux: `clear`.

**ECHO**  
Exibe texto ou variáveis no terminal.  

Exemplo:
```cmd
ECHO Olá, mundo!
```  

No Linux: `echo Olá, mundo!`.

**TYPE**  
Exibe o conteúdo de um arquivo de texto.  

Exemplo:
```cmd
TYPE config.txt
```  

No Linux: `cat config.txt`.

**COPY**  
Copia arquivos de um lugar pra outro.  

Exemplo:
```cmd
COPY arquivo.js backup\arquivo.js
```  

No Linux: `cp arquivo.js backup/arquivo.js`.

**RMDIR**  
Remove uma pasta (vazia). Use `/S` pra pastas com conteúdo.  

Exemplo:
```cmd
RMDIR pasta-vazia
RMDIR /S pasta-cheia
```  

No Linux: `rmdir pasta-vazia` ou `rm -r pasta-cheia`.

**REN**  
Renomeia arquivos ou pastas.  

Exemplo:
```cmd
REN velho.js novo.js
```  

No Linux: `mv velho.js novo.js`.

**PATH**  
Mostra ou define caminhos pra executar comandos.  

Exemplo:
```cmd
PATH
```  

No Linux: `echo $PATH`.

**Dica pra web dev:**  
Use `CD` e `DIR` pra navegar e listar projetos, `TYPE` pra checar arquivos de config (ex.: `package.json`), e `COPY` pra backups rápidos.  

**IF** (Tratamento de Erros)  
Verifica condições pra tratar erros ou tomar decisões.  

Exemplo:
```cmd
IF EXIST arquivo.js (
  ECHO Arquivo existe!
) ELSE (
  ECHO Arquivo não encontrado.
)
```  

No Linux: `if [ -f arquivo.js ]; then echo "Existe"; else echo "Não existe"; fi`.

**ERRORLEVEL** (Tratamento de Erros)  
Checa o resultado de um comando anterior.  

Exemplo:
```cmd
DEL arquivo.txt
IF %ERRORLEVEL% EQU 0 (
  ECHO Deletado com sucesso!
) ELSE (
  ECHO Falha ao deletar.
)
```  

No Linux: usa `?\` (ex.: \`if [? -eq 0 ]; then ...`).

**COMPACT** (Compactação)  
Compacta arquivos ou pastas no Windows (funcionalidade nativa).  

Exemplo:
```cmd
COMPACT /C arquivo.js
```  

No Linux: `gzip arquivo.js` ou `tar -czf arquivo.tar.gz pasta/`.

**COMPACT /U** (Descompactação)  
Descompacta arquivos previamente compactados.  

Exemplo:
```cmd
COMPACT /U arquivo.js
```  

No Linux: `gunzip arquivo.js.gz` ou `tar -xzf arquivo.tar.gz`.

**SET** (Criação de Variáveis)  
Define variáveis locais (só pra sessão atual).  

Exemplo:
```cmd
SET NOME_PROJETO=meu-site
ECHO %NOME_PROJETO%
```  

No Linux: `NOME_PROJETO=meu-site; echo $NOME_PROJETO`.

**SETX** (Criação de Variáveis Permanentes)  
Define variáveis permanentes no sistema.  

Exemplo:
```cmd
SETX API_KEY "12345"
```  

No Linux: `export API_KEY="12345" >> ~/.bashrc`.

**SDKs no CMD**  
SDKs (Software Development Kits) como Node.js ou .NET usam o CMD pra rodar comandos.  

Exemplo (Node.js):
```cmd
node --version
npm install
```  

Configuração: Adicione o caminho do SDK ao `PATH` com `SETX PATH "%PATH%;C:\caminho\do\sdk"`.  

No Linux: `node -v` ou `npm install`, com caminho em `/usr/bin`.

**Dica pra web dev:**  
Use `SET` pra variáveis temporárias (ex.: portas de servidor), `SETX` pra configs permanentes (ex.: chaves de API), e `IF`/`ERRORLEVEL` pra scripts confiáveis.  

`COMPACT` é útil pra economizar espaço, e SDKs como Node.js são essenciais pra projetos JS!

## Gerenciadores de Pacotes CLI
**Objetivo:** Entender o que são gerenciadores de pacotes CLI no Windows, como usar `winget` e `Chocolatey`, e como remover pacotes com eles.  
**O que é?**
Gerenciadores de pacotes CLI (Command Line Interface) são ferramentas que instalam, atualizam e removem programas via terminal, sem precisar de interfaces gráficas. No Windows, `winget` e `Chocolatey` são os mais populares pra devs, agilizando tarefas como instalar Node.js ou VS Code.  
**Winget**
**O que é?** Nativo do Windows (vem com Windows 10/11 recentes), criado pela Microsoft. Simples e leve.  

**Instalação:** Já vem instalado, mas se precisar, baixe da Microsoft Store ou GitHub (`winget-cli`).  

**Comandos básicos:**  
**Instalar:**
```cmd
winget install Git.Git
```
Instala o Git usando o ID do pacote.  

**Remover:**
```cmd
winget uninstall Git.Git
```
Remove o Git pelo ID. Só funciona com pacotes instalados via `winget`.  

**Pesquisar:**
```cmd
winget search git
```
Lista pacotes relacionados a "git".

**Dica:** Use `winget list` pra ver tudo instalado no PC (não só via `winget`).

**Chocolatey**
**O que é?** Ferramenta de terceiros, open-source, mais antiga e com mais pacotes (±9.500).  

**Instalação:** Via PowerShell (admin):
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```  

**Comandos básicos:**  
**Instalar:**
```cmd
choco install git
```
Instala o Git (nome do pacote, não ID).  

**Remover:**
```cmd
choco uninstall git
```
Remove o Git. Só funciona com pacotes instalados via `Chocolatey`.  

**Pesquisar:**
```cmd
choco search git
```
Busca pacotes com "git".

**Dica:** Use `choco list -localonly` pra ver pacotes instalados com `Chocolatey`.

**Comparação:**  
`winget`: Simples, nativo, menos pacotes. Bom pra iniciantes.  

`Chocolatey`: Mais opções, precisa instalar, ótimo pra automação (ex.: DevOps).

**Dica pra web dev:**  
Use `winget` pra installs rápidas (ex.: `winget install Microsoft.VisualStudioCode`).  

Use `Chocolatey` pra setups complexos (ex.: `choco install nodejs docker`).  

Sempre cheque o ID/nome do pacote antes de remover! :3


