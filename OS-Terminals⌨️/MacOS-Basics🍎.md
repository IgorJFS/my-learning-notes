# macOS Terminal Commands
**Objetivo:** Dominar o uso do Terminal no macOS, aprendendo comandos essenciais e avançados, com comparações diretas ao Linux pra entender as semelhanças e diferenças.  
Introdução ao Terminal no macOS
O **Terminal** no macOS é uma interface de linha de comando baseada em **bash** (até macOS Mojave) e **zsh** (a partir do Catalina, 2019).  

Ele acessa o núcleo Unix do macOS (derivado do BSD), então muitos comandos são iguais ou parecidos com os do Linux.  

Diferenças pro Linux:  
macOS usa **BSD Unix** (não POSIX puro), então alguns comandos (ex.: `ps`, `top`) têm flags diferentes.  

Sistema de arquivos: macOS tem restrições (ex.: SIP - System Integrity Protection) e usa APFS/HFS+ em vez de ext4.  

Ferramentas nativas são limitadas; usamos **Homebrew** pra suprir.

Configurando o Terminal
Abra o Terminal: **Finder > Aplicativos > Utilitários > Terminal** ou use Spotlight (`Cmd + T`).  

Mude pra **zsh** (se não for padrão):
```bash
chsh -s /bin/zsh
```  
**Linux equivalente:**
```bash
chsh -s /bin/zsh
```  
Mesma lógica, mas no Linux você pode escolher **bash**, **fish**, etc., dependendo da distro.

Personalize o **.zshrc**:
```bash
nano ~/.zshrc
# Exemplo: Adiciona alias
alias ll='ls -lah'
```  
Salve (`Ctrl + O`, Enter, `Ctrl + X`) e recarregue:
```bash
source ~/.zshrc
```  

**Linux equivalente:**
```bash
nano ~/.bashrc  # ou ~/.zshrc se usar zsh
alias ll='ls -lah'
source ~/.bashrc
```  
Diferença: Linux usa **.bashrc** por padrão pro bash; macOS usa **.zshrc** pro zsh.

Comandos Básicos
Navegação no Sistema de Arquivos
**Listar arquivos (`ls`):**
```bash
ls -l  # Lista com detalhes
ls -a  # Mostra ocultos (.zshrc, etc.)
ls -lah  # Detalhes + ocultos + tamanho legível
```  
**Linux equivalente:**
```bash
ls -l
ls -a
ls -lah
```  
Mesmos comandos, mesmas flags. No Linux, **ls** pode ter cores diferentes dependendo da distro.

**Mudar diretório (`cd`):**
```bash
cd /Users/igor/Documents
cd ..  # Volta um nível
cd ~  # Vai pra home (/Users/igor)
```  
**Linux equivalente:**
```bash
cd /home/igor/Documents
cd ..
cd ~
```  
Diferença: macOS usa **/Users** pro home; Linux usa **/home**.

**Caminho atual (`pwd`):**
```bash
pwd  # Ex.: /Users/igor/projects
```  
**Linux equivalente:**
```bash
pwd
```  
Idêntico.

Gerenciamento de Arquivos
**Criar arquivo (`touch`):**
```bash
touch meu_arquivo.txt
```  
**Linux equivalente:**
```bash
touch meu_arquivo.txt
```  
Igual.

**Criar diretório (`mkdir`):**
```bash
mkdir minha_pasta
mkdir -p pasta/subpasta  # Cria hierarquia
```  
**Linux equivalente:**
```bash
mkdir minha_pasta
mkdir -p pasta/subpasta
```  
Idêntico.

**Copiar (`cp`):**
```bash
cp arquivo.txt copia.txt
cp -r pasta nova_pasta  # Copia diretório
```  
**Linux equivalente:**
```bash
cp arquivo.txt copia.txt
cp -r pasta nova_pasta
```  
Igual.

**Mover/Renomear (`mv`):**
```bash
mv arquivo.txt novo_nome.txt
mv pasta /Users/igor/backup/
```  
**Linux equivalente:**
```bash
mv arquivo.txt novo_nome.txt
mv pasta /home/igor/backup/
```  
Mesma lógica, só muda o caminho base.

**Remover (`rm`):**
```bash
rm arquivo.txt
rm -r pasta  # Remove diretório
rm -rf pasta  # Força remoção sem confirmação
```  
**Linux equivalente:**
```bash
rm arquivo.txt
rm -r pasta
rm -rf pasta
```  
Idêntico. **Cuidado:** `-rf` é perigoso em ambos!

Visualizar Arquivos
**Ler arquivo (`cat`):**
```bash
cat arquivo.txt
```  
**Linux equivalente:**
```bash
cat arquivo.txt
```  
Igual.

**Paginação (`less`):**
```bash
less arquivo.txt  # Navegue com setas, saia com 'q'
```  
**Linux equivalente:**
```bash
less arquivo.txt
```  
Idêntico.

**Cabeçalho (`head`)/Final (`tail`):**
```bash
head -n 5 arquivo.txt  # Primeiras 5 linhas
tail -n 5 arquivo.txt  # Últimas 5 linhas
```  
**Linux equivalente:**
```bash
head -n 5 arquivo.txt
tail -n 5 arquivo.txt
```  
Mesmos comandos.

Gerenciamento de Pacotes com Homebrew
**Homebrew** é o gerenciador de pacotes do macOS, como **apt** ou **yum** no Linux.  

Instale:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```  

Configure (se necessário):
```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
source ~/.zshrc
```  

**Comandos principais:**
```bash
brew install git  # Instala git
brew update  # Atualiza brew
brew upgrade  # Atualiza pacotes
brew list  # Lista instalados
brew uninstall git  # Remove git
```  
**Linux equivalente (Ubuntu):**
```bash
sudo apt update
sudo apt install git
sudo apt upgrade
dpkg --list | grep git
sudo apt remove git
```  
Diferença: **apt** usa **sudo** e é nativo; **brew** é instalado pelo usuário e não precisa **sudo**.

Gerenciamento de Processos
**Listar processos (`ps`):**
```bash
ps aux  # Lista todos com detalhes
ps aux | grep python  # Filtra processos python
```  
**Linux equivalente:**
```bash
ps aux
ps aux | grep python
```  
Mesma sintaxe, mas macOS (BSD) pode mostrar menos detalhes que Linux.

**Monitorar em tempo real (`top`):**
```bash
top  # Sai com 'q'
```  
**Linux equivalente:**
```bash
top
```  
Igual, mas macOS **top** tem interface mais simples. Linux pode usar **htop** (instalável via **apt**).

**Matar processo (`kill`):**
```bash
kill 1234  # Mata processo com PID 1234
kill -9 1234  # Força matar
```  
**Linux equivalente:**
```bash
kill 1234
kill -9 1234
```  
Idêntico.

Rede
**Verificar conectividade (`ping`):**
```bash
ping -c 4 google.com  # Envia 4 pacotes
```  
**Linux equivalente:**
```bash
ping -c 4 google.com
```  
Igual.

**Informações de rede (`ifconfig`):**
```bash
ifconfig  # Lista interfaces
ifconfig en0  # Detalhes da interface Wi-Fi
```  
**Linux equivalente:**
```bash
ip addr  # Ou ifconfig, se instalado
ip link show eth0
```  
Diferença: Linux prefere **ip**; **ifconfig** é mais antigo.

**Testar portas (`nc`):**
```bash
nc -zv localhost 8080  # Verifica se porta 8080 tá aberta
```  
**Linux equivalente:**
```bash
nc -zv localhost 8080
```  
Idêntico, mas **nc** precisa estar instalado no Linux (`apt install netcat`).

Comandos Avançados
Pesquisar Arquivos (`find` e `\mdfind`)
**find:**
```bash
find /Users/igor -name "*.txt"  # Busca arquivos .txt
find . -type d -name "backup"  # Busca diretórios chamados backup
```  
**Linux equivalente:**
```bash
find /home/igor -name "*.txt"
find . -type d -name "backup"
```  
Mesma lógica, só muda o caminho base.

**mdfind (exclusivo macOS):**
```bash
mdfind "kMDItemKind == 'Text'"  # Busca arquivos de texto via Spotlight
mdfind "from:Igor"  # Busca emails do Igor
```  
**Linux equivalente:**
```bash
locate *.txt  # Após atualizar com 'sudo updatedb'
```  
Diferença: **mdfind** usa o Spotlight (índice macOS); Linux usa **locate** (menos poderoso).

Redirecionamento e Pipes
**Redirecionar saída:**
```bash
ls -l > lista.txt  # Salva saída
echo "teste" >> arquivo.txt  # Adiciona ao final
```  
**Linux equivalente:**
```bash
ls -l > lista.txt
echo "teste" >> arquivo.txt
```  
Igual.

**Pipes (`|`):**
```bash
ps aux | grep python  # Filtra python
ls -l | wc -l  # Conta arquivos
```  
**Linux equivalente:**
```bash
ps aux | grep python
ls -l | wc -l
```  
Idêntico.

Scripts no Terminal
Crie um script:
```bash
nano myscript.sh
# Conteúdo:
#!/bin/zsh
echo "Olá, Igor!"
ls -lah
```  
Torne executável:
```bash
chmod +x myscript.sh
./myscript.sh
```  

**Linux equivalente:**
```bash
nano myscript.sh
# Conteúdo:
#!/bin/bash
echo "Olá, Igor!"
ls -lah
chmod +x myscript.sh
./myscript.sh
```  
Diferença: macOS usa **zsh** por padrão; Linux usa **bash**.

Exemplo Prático: Configurando um Projeto
Cenário: Você quer criar uma pasta, instalar **Node.js** via brew, e rodar um script.
```bash
mkdir meu_projeto
cd meu_projeto
brew install node
node --version
touch server.js
echo "console.log('Olá, macOS!')" > server.js
node server.js
```  

**Linux equivalente (Ubuntu):**
```bash
mkdir meu_projeto
cd meu_projeto
sudo apt update
sudo apt install nodejs npm
node --version
touch server.js
echo "console.log('Olá, Linux!')" > server.js
node server.js
```  
Diferença: **brew** é mais simples no macOS; **apt** exige **sudo** e instala **npm** separado.

