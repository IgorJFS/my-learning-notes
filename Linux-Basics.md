# Linux Basics: Comandos Essenciais 🐧
Linux é um sistema operacional poderoso, usado em servidores, desenvolvimento e até no WSL (Windows Subsystem for Linux). Aqui estão comandos básicos pra navegar, gerenciar arquivos, instalar pacotes e editar código.
## Navegação e Exploração
``pwd``:
Mostra o caminho absoluto do diretório atual.
Exemplo: `/home/usuario`  
Útil pra saber "onde estou".

``ls (equivalente: dir no Windows)``:
Lista arquivos e diretórios. No Linux, usa-se `ls`, mas como vamos rodar no WSL ou terminal Linux, é assim:
Exemplo: `ls -l` (lista com detalhes)  
Não funciona nativo no CMD/PowerShell, mas no Linux/WSL é essencial.

``cd``:
Muda de diretório.
Exemplo: `cd /var/www` (vai pra `/var/www`)  
`cd ..` sobe um nível, `cd ~` vai pro diretório home.

## Gerenciamento de Arquivos
``touch``:
Cria um arquivo vazio.
Exemplo: `touch notas.txt`  
Se o arquivo já existir, só atualiza o timestamp.

``mkdir``:
Cria um diretório.
Exemplo: `mkdir projetos`  
`mkdir -p caminho/novo` cria subdiretórios se necessário.

``cp``:
Copia arquivos ou diretórios.
Exemplo: `cp arquivo.txt copia.txt`  
`cp -r pasta1 pasta2` copia uma pasta inteira.

``mv``:
Move ou renomeia arquivos/diretórios.
Exemplo: `mv velho.txt novo.txt` (renomeia)  
`mv arquivo.txt /outro/caminho/` (move).

``rm``:
Remove arquivos ou diretórios.
Exemplo: `rm arquivo.txt`  
`rm -r pasta` remove uma pasta e tudo dentro (cuidado!).

``cat``:
Mostra o conteúdo de um arquivo ou concatena arquivos.
Exemplo: `cat notas.txt` (mostra o texto)  
`cat file1.txt file2.txt > novo.txt` junta dois arquivos em um novo.

## Informações do Sistema
``whoami``:
Mostra o usuário atual.
Exemplo: `usuario`  
Simples e direto.

``uname``:
Mostra infos do sistema.
Exemplo: `uname -a` (mostra tudo, como "Linux 5.15...")  
Bom pra checar a versão do kernel.

``df -h``
Mostra o espaço em disco em formato legível.
Exemplo: `df -h` (exibe em GB/MB)  
Útil pra ver quanto espaço sobrou.

## Busca e Permissões
``grep``:
Busca por texto dentro de arquivos ou saída de comandos.
Exemplo: `grep "usuario" arquivo.txt` (mostra linhas com "usuario")  
`ls -l | grep "txt"` filtra a saída do `ls` pra linhas com "txt".

``chmod``:
Altera permissões de arquivos/diretórios (leitura, escrita, execução).
Exemplo: `chmod 755 script.sh` (dono executa, outros leem)  
`chmod +x script.sh` adiciona permissão de execução pra todos.

## Instalação e Gerenciamento de Pacotes
``apt update``:
Atualiza a lista de pacotes disponíveis (no Ubuntu/Debian).
Exemplo: `sudo apt update`  
Use `sudo` pra rodar como administrador.

``apt install``
Instala pacotes/softwares.
Exemplo: `sudo apt install nodejs` (instala Node.js)  
`sudo apt install -y` evita confirmações manuais.

``apt remove``:
Remove pacotes, mantendo arquivos de config.
Exemplo: `sudo apt remove nodejs`  
`apt purge` remove tudo, incluindo configs.

## Edição de Arquivos
``vi ou vim``:
Editor de texto no terminal (vim é a versão melhorada).
Exemplo: `vi arquivo.txt` (abre o arquivo)  
Modos:  
`i` entra no modo de inserção (edita).  

`Esc` sai do modo de inserção.  

`:w` salva, `:q` sai, `:wq` salva e sai, `:q!` sai sem salvar.

Dica: `vim arquivo.txt` é mais comum hoje (cores e mais recursos).

## Extras pra Back-end
``ps``:
Lista processos rodando.
Exemplo: `ps aux` (mostra todos os processos detalhados)  
Útil pra checar se seu servidor Node.js tá ativo.

``kill``:
Encerra processos pelo ID (PID).
Exemplo: `kill 1234` (mata o processo com PID 1234)  
`kill -9` força o encerramento.

## Dica Prática
Combine comandos com `&&` pra rodar um após o outro:
Exemplo: `mkdir temp && cd temp && touch teste.txt`  
Cria pasta, entra nela e cria um arquivo, tudo de uma vez!
