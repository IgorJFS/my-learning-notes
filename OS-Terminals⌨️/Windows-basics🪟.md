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

