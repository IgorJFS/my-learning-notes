# Web Assets Quick Reference
List of useful CDNs. Perfect for copy and paste in web projects.

Lista de CDNs úteis. Ideal para copiar e colar em projetos web.

## CDNs - CSS/JavaScripts e mais
**Bootstrap 5.3.3 (JS Included), Jquery 3.7.1, Axios 1.7.2...(more soon)**


**Bootstrap 5.3.3**


```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" 
rel="stylesheet" 
integrity="sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH" 
crossorigin="anonymous">
```
**Bootstrap 5.3.3**(includes Popper.js)

```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-YvpcrYf0tY3lHB60NNkmXc5s9fDVZLESaAA55NDzOxhy9GkcIdslK1eN7N6jIeHz" crossorigin="anonymous"></script>
```
## **jQuery 3.7.1:**
```html
<script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
```

## **🌐Axios 1.7.2:**
```html
<script src="https://cdn.jsdelivr.net/npm/axios@1.7.2/dist/axios.min.js" integrity="sha256-MfXcwF9U5mMSEb0S2PfsLCOXOLPW6CSzUhjTGpFjvgM=" crossorigin="anonymous"></script>
```

## **Node.JS**



[https://nodejs.org/en/download](https://nodejs.org/en/download)

## Máquina Virtual / Virtual Machine(VM) 💻
[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

## Instalação de VM Linux 🐧
**Ubuntu 22.04 LTS ISO (Máquina Virtual)**:

Download: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop) 

Use com VirtualBox ou VMware. Após baixar, configure a VM com pelo menos 2GB RAM e 20GB de disco.

## Instalação do Git 🔧
**Git para Windows (via instalador)**:

Download: [https://git-scm.com/download/win](https://git-scm.com/download/win) (leva ao instalador mais recente, ex.: `Git-2.44.0-64-bit.exe`)  

Inclui Git Bash. Execute o `.exe` e siga as instruções.

**Git para Linux (via terminal)**: 
Comando: `sudo apt install git` (Ubuntu/Debian)  
Rode no terminal da sua VM Linux após `sudo apt update`.


## WSL e Ubuntu no Windows (via PowerShell):
Execute os comandos abaixo no PowerShell como administrador para instalar o WSL (Windows Subsystem for Linux) e o Ubuntu:
powershell

**Habilita o WSL**
```html
wsl --install
```
**Após reiniciar o PC, instala o Ubuntu (versão padrão, ex.: 22.04)**
```html
wsl --install -d Ubuntu
```
Nota: Após o primeiro comando, reinicie o Windows. Depois, rode o segundo comando. Na primeira execução do Ubuntu, configure um usuário e senha.