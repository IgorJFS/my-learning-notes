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

``rm`` e ``rmdir``:
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

- **Tipos de Permissões**: No Linux, cada arquivo tem permissões pra três grupos:  
  - **Dono (user)**: Quem criou o arquivo.  
  - **Grupo (group)**: Usuários no mesmo grupo do dono.  
  - **Outros (others)**: Todo mundo que não é dono nem do grupo.  
- **Permissões Básicas**:  
  - **Leitura (r)**: Permite ler o arquivo (`4` em número).  
  - **Escrita (w)**: Permite editar o arquivo (`2` em número).  
  - **Execução (x)**: Permite rodar o arquivo como programa (`1` em número).  
- **Como os Números Funcionam**:  
  - Usa-se uma soma pra cada grupo (dono, grupo, outros).  
  - Ex.: `7` = leitura (4) + escrita (2) + execução (1).  
  - Ex.: `5` = leitura (4) + execução (1).  
  - Ex.: `4` = só leitura.  
  - Ordem: **dono-grupo-outros**. Então `755` significa:  
    - Dono: 7 (rwx), Grupo: 5 (r-x), Outros: 5 (r-x).  
- **Exemplo Prático**:  
  - `chmod 644 arquivo.txt`: Dono lê/escreve (6), grupo e outros só leem (4).  
  - `chmod 700 script.sh`: Só o dono tem todos os acessos (7), grupo e outros nada (0).  
- **Dica Visual**: Veja com `ls -l`. Ex.: `-rwxr-xr-x` significa dono (rwx), grupo (r-x), outros (r-x).  

## Adicionar um usuário ao sistema:
No terminal Linux (VM ou WSL), crie um novo usuário com:
```bash
sudo adduser nome_do_usuario
```
Você será solicitado a definir uma senha e preencher algumas informações opcionais (nome completo, telefone, etc.).

**Criar um grupo**:
Crie um grupo para organizar usuários com:
```bash
sudo groupadd nome_do_grupo
```
**Adicionar um usuário a um grupo**:
Associe o usuário ao grupo criado com:
```bash
sudo usermod -aG nome_do_grupo nome_do_usuario
```
**-aG**: Adiciona o usuário ao grupo sem remover de outros grupos existentes.

Verifique com: `groups nome_do_usuario`.

**Dar permissões ao grupo para arquivos/scripts**:
Para permitir que o grupo acesse e edite arquivos ou scripts, ajuste as permissões com:
```bash
# Define o grupo como dono do arquivo/pasta
sudo chgrp nome_do_grupo /caminho/para/arquivo_ou_pasta
# Dá permissões de leitura, escrita e execução ao grupo (rwxrw-r--)
sudo chmod g+rwx /caminho/para/arquivo_ou_pasta
```

**Sobre o chown**:  
`sudo chown nome_do_usuario:nome_do_grupo arquivo` muda o usuário e o grupo dono.

`sudo chown :nome_do_grupo arquivo` muda apenas o grupo.

**Exemplo prático**: Se você tem um script `meu_script.sh`:
```bash
sudo chown :nome_do_grupo meu_script.sh
sudo chmod g+rwx meu_script.sh
```

Use `ls -l` para verificar as permissões (ex.: `rwxrw-r--`).

## **Compactação e descompactação de arquivos com tar**:
O comando `tar` é usado para criar e extrair arquivos compactados no Linux:
```bash
# Compactar uma pasta ou arquivos em .tar.gz (com gzip)
tar -czf arquivo_compactado.tar.gz /caminho/para/pasta_ou_arquivo
# Descompactar um arquivo .tar.gz
tar -xzf arquivo_compactado.tar.gz
# Listar conteúdo de um .tar.gz sem descompactar
tar -tzf arquivo_compactado.tar.gz
```
**Opções do tar**:  
`-c`: Cria um novo arquivo tar.

`-z`: Usa gzip para compactar (`.tar.gz`).

`-f`: Especifica o nome do arquivo.

`-x`: Extrai o conteúdo.

`-t`: Lista o conteúdo.

**Exemplo prático**:  
Compactar: `tar -czf backup.tar.gz ./minha_pasta`

Descompactar: `tar -xzf backup.tar.gz`

**Nota**: Sem `-z`, cria/extrai apenas `.tar` (sem compressão).

## Registro de logs com Syslog:
O **Syslog** é um sistema padrão no Linux para registrar mensagens de log geradas pelo sistema e por aplicativos. Ele é essencial para monitorar eventos, depurar problemas e auditar atividades.
**Onde os logs ficam**:  
Em sistemas como Ubuntu, os logs geralmente estão em `/var/log/syslog` (ou `/var/log/messages` em outras distros).

Outros arquivos comuns: `/var/log/auth.log` (autenticação), `/var/log/kern.log` (kernel).

**Como visualizar logs**:
```bash
# Ver todo o conteúdo do syslog
cat /var/log/syslog
# Ver em tempo real (atualiza conforme novos logs chegam)
tail -f /var/log/syslog
# Filtrar logs específicos (ex.: do Cron)
grep "CRON" /var/log/syslog
```

## Estrutura de uma linha de log:  
Exemplo: `Mar 28 10:00:00 hostname CRON[1234]: (user) CMD (comando)`

Campos: data/hora, nome do host, serviço/processo, PID (entre colchetes), mensagem.

**Configuração**:  
O arquivo de configuração principal é `/etc/syslog.conf` ou `/etc/rsyslog.conf` (depende da implementação, como `rsyslog` no Ubuntu).

Exemplo de linha: `*.info /var/log/messages` (registra mensagens de nível "info" ou superior).

**Níveis de severidade**:  
De 0 (emergência) a 7 (debug): emerg, alert, crit, err, warning, notice, info, debug.

Use `logger` pra testar:
```bash
logger -p user.info "Teste de log do Igor"
```

**Exemplo prático**:  
Verificar falhas de login:
```bash
sudo cat /var/log/auth.log | grep "FAILED"
```

Adicionar um log customizado e verificar:
```bash
logger "Backup concluído"
tail -n 10 /var/log/syslog
```

**Dicas**:  
Use `sudo` para acessar logs, pois são protegidos.

Rotação de logs é gerenciada pelo `logrotate` (config em `/etc/logrotate.conf`).

Para mais detalhes, veja `man syslog` ou `man rsyslogd`.

## Regex

**Componentes básicos**
Retomando um pouco do que abordamos em vídeo, uma regex possui dois componentes básicos. Vamos conferir!

Caracteres literais: correspondem exatamente aos caracteres no texto. Exemplo: abc corresponde à string "abc" no texto.

Metacaracteres: caracteres especiais que possuem significados específicos.

Os principais metacaracteres são:

```.``` (ponto): corresponde a qualquer caractere, exceto quebras de linha. Exemplo: a.b corresponde a "aab", "axb", "a1b", etc.

```^``` (circunflexo): corresponde ao início de uma linha. Exemplo: ^abc corresponde a "abc" apenas no início de uma linha.

```$``` (cifrão): corresponde ao final de uma linha. Exemplo: abc$ corresponde a "abc" apenas no final de uma linha.

```[]```: conjunto de caracteres. Corresponde a qualquer caractere dentro dos colchetes. Exemplo: [aeiou] corresponde a qualquer vogal.

```[^]```: negação dentro de colchetes. Corresponde a qualquer caractere que não esteja dentro dos colchetes. Exemplo: [^a-z] corresponde a qualquer caractere que não seja uma letra minúscula.

```*```: corresponde a zero ou mais repetições do caractere anterior. Exemplo: a* corresponde a "", "a", "aa", "aaa", etc.

```+```: corresponde a uma ou mais repetições do caractere anterior. Exemplo: a+ corresponde a "a", "aa", "aaa", etc., mas não a "" (vazio).
```?```: corresponde a zero ou uma repetição do caractere anterior. Exemplo: a? corresponde a "" ou "a".

```{n,m}```: corresponde entre n e m repetições do caractere anterior. Exemplo: a{2,4} corresponde a "aa", "aaa" ou "aaaa".

```|```: alternativa. Corresponde a qualquer um dos padrões à esquerda ou à direita do |. Exemplo: cat|dog corresponde a "cat" ou "dog".

```()```: usado para agrupar partes de um padrão ou criar grupos de captura. Exemplo: A expressão regular fail(ed)? busca tanto a palavra "fail" quanto "failed", sendo o "ed" opcional graças ao uso dos parênteses para agrupar e do ponto de interrogação ? para tornar essa parte do padrão opcional.
Classes de caracteres:


```\d```: corresponde a qualquer dígito (equivalente a [0-9]).

```\w```: corresponde a qualquer caractere alfanumérico (letras e números) e o sublinhado (equivalente a [a-zA-Z0-9_]).

```\s```: corresponde a qualquer caractere de espaço em branco (como espaço, tabulação e nova linha).

```\D```: corresponde a qualquer caractere que não seja um dígito.

```\W```: corresponde a qualquer caractere que não seja alfanumérico ou sublinhado.

```\S```: corresponde a qualquer caractere que não seja espaço em branco.
Expressões regulares estendidas vs. básicas
Ainda podemos classificar as expressões em dois tipos, são eles:

Expressões regulares básicas: são mais simples e algumas metacaracteres (como ?, +, {}) têm que ser escapados com uma barra invertida (\).

Expressões regulares estendidas: oferecem mais funcionalidades, como não precisar escapar os caracteres ```?```, ```+```, ```{}```, e ```|```.

Em resumo, podemos dizer que regex é essencialmente uma linguagem para criar padrões que correspondem a sequências específicas de caracteres dentro de uma string.

Mas você não precisa decorar todos os caracteres, pois existem diversas ferramentas gratuitas disponíveis na web para auxiliar na criação e validação de expressões regulares. Entre as mais populares estão:

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

## Ordenar dados com sort:
O comando `sort` organiza linhas de um arquivo em ordem alfabética ou numérica:
```bash
# Ordena um arquivo e exibe no terminal
sort nome_do_arquivo.txt
# Ordena e salva em outro arquivo
sort nome_do_arquivo.txt > arquivo_ordenado.txt
# Ordena numericamente
sort -n nome_do_arquivo.txt
```
**Exemplo**: Se `numeros.txt` tem "3, 1, 2", `sort -n numeros.txt` retorna "1, 2, 3".

## Comparar arquivos com diff:
O comando `diff` mostra diferenças entre dois arquivos:
```bash
diff arquivo1.txt arquivo2.txt
```
**Saída**: Linhas com `<` (só em arquivo1) ou `>` (só em arquivo2).

**Exemplo**: Se `arquivo1.txt` tem "ola" e `arquivo2.txt` tem "oi", o resultado mostra as linhas diferentes.

Use `diff -u` para um formato mais legível (unificado).

## Monitoramento de Rede no Linux:
No Linux, você pode monitorar a rede para verificar conexões, tráfego, portas abertas e desempenho usando ferramentas nativas. Isso é útil para depurar problemas de conectividade, segurança ou desempenho em projetos.
**Ver interfaces de rede**:
```bash
# Lista interfaces de rede e status
ip link show
# Alternativa mais detalhada
ifconfig
```

**Checar endereços IP**:
```bash
# Mostra IPs associados às interfaces
ip addr show
```

**Monitorar tráfego em tempo real**:  
Use o `iftop` (instale com `sudo apt install iftop`):
```bash
sudo iftop
```
Mostra uso de banda por conexão (como um "top" para rede).

**Listar portas abertas e conexões**:  
Use o `netstat` (instale com `sudo apt install net-tools` se necessário):
```bash
# Lista todas as portas abertas e conexões
netstat -tuln
# Inclui processos associados
netstat -tulnp
```

Alternativa moderna com `ss`:
```bash
# Lista portas e conexões
ss -tuln
```

**Testar conectividade**:
```bash
# Pinga um host pra verificar alcance
ping google.com
# Testa uma porta específica (ex.: 80)
nc -zv google.com 80
```
`nc` (netcat) pode precisar ser instalado: `sudo apt install netcat`.

**Capturar pacotes**:  
Use o `tcpdump` (instale com `sudo apt install tcpdump`):
```bash
# Captura tráfego na interface eth0
sudo tcpdump -i eth0
# Salva em um arquivo pra análise
sudo tcpdump -i eth0 -w captura.pcap
```

**Exemplo prático**:  
Verificar se um servidor web local está ativo:
```bash
ss -tuln | grep ":80"
```

Monitorar tráfego de uma API:
```bash
sudo tcpdump -i any port 443 -c 100
```
Captura 100 pacotes na porta 443 (HTTPS).

**Dicas**:  
Use `sudo` pra comandos que acessam dados privilegiados.

Para análise avançada, instale o **Wireshark**: `sudo apt install wireshark`.

Veja mais detalhes com `man <comando>` (ex.: `man tcpdump`).

## Gerenciamento de Disco e Armazenamento no Linux:
No Linux, gerenciar discos e armazenamento envolve verificar espaço, listar dispositivos, montar/desmontar unidades e formatar partições. Isso é essencial para administrar sistemas ou projetos que dependem de arquivos.
**Verificar uso de disco**:
```bash
# Mostra espaço usado/livre em discos montados
df -h
```
`-h`: Formato legível (ex.: GB, MB).

**Listar dispositivos e partições**:
```bash
# Lista discos e partições
lsblk
# Detalhes sobre dispositivos de bloco
sudo fdisk -l
```
`fdisk -l` requer permissões de root.

**Checar espaço de arquivos/pastas**:
```bash
# Mostra tamanho de uma pasta e subpastas
du -sh /caminho/para/pasta
```
`-s`: Soma tudo; `-h`: Formato legível.

**Montar uma unidade**:
```bash
# Monta um dispositivo (ex.: pendrive) em um diretório
sudo mount /dev/sdb1 /mnt
```
`/dev/sdb1`: Partição (verifique com `lsblk`).

`/mnt`: Ponto de montagem (crie com `mkdir /mnt` se necessário).

**Desmontar uma unidade**:
```bash
sudo umount /mnt
```

**Formatar uma partição**:  
Exemplo (formato FAT32):
```bash
sudo mkfs.vfat -F 32 /dev/sdb1
```

**Cuidado**: Isso apaga todos os dados! Confirme o dispositivo com `lsblk`.

**Gerenciar espaço em disco**:  
Ver arquivos grandes:
```bash
# Lista os 10 maiores arquivos em /caminho
du -ah /caminho | sort -rh | head -n 10
```

Limpar pacotes órfãos (Ubuntu):
```bash
sudo apt autoremove
```

**Exemplo prático**:  
Verificar e limpar espaço:
```bash
df -h
du -sh /var/cache
sudo apt clean
```

Montar um pendrive e copiar arquivos:
```bash
lsblk
sudo mount /dev/sdc1 /mnt
cp arquivo.txt /mnt
sudo umount /mnt
```

**Dicas**:  
Use `sudo` para operações em discos.

Para gerenciamento avançado, instale o `gparted`: `sudo apt install gparted`.

## Monitoramento de desempenho com iostat**:
O `iostat` é uma ferramenta no Linux para monitorar o desempenho da CPU e dos dispositivos de entrada/saída (I/O), como discos. Ele ajuda a identificar gargalos no sistema, seja por uso intenso da CPU ou por operações lentas de disco.
**Instalação**:  
Geralmente vem no pacote `sysstat`. Instale com:
```bash
sudo apt install sysstat
```

**Uso básico**:
```bash
# Mostra estatísticas desde o boot
iostat
# Atualiza a cada 2 segundos
iostat 2
```

**Saída explicada (CPU)**:
A seção de CPU mostra como o processador está sendo usado:
```bash
%user   %nice   %system   %iowait   %steal   %idle
3.50    0.10    1.20      0.30      0.00     94.90
```
**%user**: Percentual de tempo da CPU gasto em processos de usuário (ex.: seus scripts ou apps).

**%nice**: Percentual de tempo em processos de usuário com prioridade ajustada (via `nice`).

**%system**: Percentual de tempo em processos do sistema/kernel (ex.: gerenciamento de hardware).

**%iowait**: Percentual de tempo que a CPU ficou ociosa esperando operações de I/O (ex.: leitura/escrita em disco).

**%steal**: Percentual de tempo "roubado" por máquinas virtuais (relevante em VMs, como WSL ou servidores).

**%idle**: Percentual de tempo que a CPU ficou ociosa (sem nada pra fazer).

**Saída explicada (Dispositivos)**:
A seção de dispositivos mostra o desempenho de discos (ex.: `sda`, `sdb`, `sdc`):
```bash
Device   tps    kB_read/s   kB_wrtn/s   kB_read   kB_wrtn
sda     5.20   120.50      45.30       123456    7890
sdb     2.10   50.70       10.20       56789     1234
```
**Device**: Nome do dispositivo (ex.: `sda` é o primeiro disco, `sdb` o segundo, etc.).

**tps**: Transações por segundo (operações de leitura/escrita).

**kB_read/s**: Quilobytes lidos por segundo.

**kB_wrtn/s**: Quilobytes escritos por segundo.

**kB_read**: Total de quilobytes lidos desde o boot.

**kB_wrtn**: Total de quilobytes escritos desde o boot.

**Exemplo prático**:  
Monitorar em tempo real:
```bash
iostat -x 2
```
`-x`: Mostra estatísticas estendidas (mais detalhes).

Identificar gargalo:  
Se `%iowait` estiver alto (ex.: >20%), o disco pode estar lento.

Se `kB_read/s` ou `kB_wrtn/s` de `sda` estiverem altos, o disco está muito ativo.

**Dicas**:  
Combine com `df -h` ou `du` pra correlacionar uso de disco com I/O.

Use `iostat -d` pra focar só em dispositivos, sem CPU.


## Gerenciamento de serviços com Systemd**:
O **Systemd** é o sistema de inicialização e gerenciamento de serviços padrão em muitas distribuições Linux modernas (como Ubuntu). Ele substitui o antigo `init` e controla serviços, montagens, logs e outros processos do sistema.
**O que é**:  
Um "gerente" que inicia o sistema, gerencia unidades (serviços, dispositivos, etc.) e mantém o estado operacional.

Usa arquivos de configuração chamados "units" (ex.: `.service`, `.timer`).

**Comandos principais**:  
Ver status de um serviço:
```bash
systemctl status nome_do_servico
```
Ex.: `systemctl status ssh` (mostra se o SSH está ativo).

Iniciar um serviço:
```bash
sudo systemctl start nome_do_servico
```

Parar um serviço:
```bash
sudo systemctl stop nome_do_servico
```

Habilitar na inicialização:
```bash
sudo systemctl enable nome_do_servico
```

Desabilitar na inicialização:
```bash
sudo systemctl disable nome_do_servico
```

Reiniciar um serviço:
```bash
sudo systemctl restart nome_do_servico
```

Recarregar configurações sem parar:
```bash
sudo systemctl reload nome_do_servico
```

**Listar serviços**:
```bash
# Lista todos os serviços ativos
systemctl list-units --type=service
# Lista serviços habilitados
systemctl list-unit-files --type=service
```

**Logs com journalctl**:  
O Systemd usa o `journald` pra gerenciar logs:
```bash
# Ver todos os logs do sistema
journalctl
# Logs de um serviço específico
journalctl -u nome_do_servico
# Logs em tempo real
journalctl -f
```

**Arquivos de configuração**:  
Localizados em `/etc/systemd/system/` (customizados) ou `/lib/systemd/system/` (padrão).

Exemplo de um arquivo `.service`:
```ini
[Unit]
Description=Meu Serviço de Exemplo
After=network.target
[Service]
ExecStart=/usr/bin/meu_script.sh
Restart=always
[Install]
WantedBy=multi-user.target
```

Após editar, recarregue com:
```bash
sudo systemctl daemon-reload
```

**Exemplo prático**:  
Criar e gerenciar um serviço simples:  
Crie `/etc/systemd/system/meu-servico.service` com o conteúdo acima.

Ative e inicie:
```bash
sudo systemctl daemon-reload
sudo systemctl enable meu-servico
sudo systemctl start meu-servico
```

Verifique:
```bash
systemctl status meu-servico
journalctl -u meu-servico
```

**Outros recursos**:  
**Timers**: Substituem o Cron para agendamentos. Ex.: `systemctl list-timers`.

**Reboot/Shutdown**:
```bash
sudo systemctl reboot
sudo systemctl poweroff
```

**Dicas**:  
Use `systemctl is-active nome_do_servico` pra checar se está rodando (retorna "active" ou "inactive").

Para debug, veja logs detalhados com `journalctl -xe`.

Documentação: `man systemctl` ou `man systemd.service`.



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
