# Gerenciamento de Pacotes em Python

O gerenciamento de dependências é essencial para criar projetos Python organizados, reproduzíveis e fáceis de compartilhar. Este guia explora as principais ferramentas e boas práticas nesse campo.

---

## 1. Pip - O Gerenciador Básico

Pip é o instalador de pacotes padrão do Python, incluído automaticamente nas instalações mais recentes.

### 1.1 Comandos Básicos

```bash
# Instalação de pacote
pip install pacote

# Instalação de versão específica
pip install pacote==1.2.3

# Instalação com requisitos de versão
pip install "pacote>=1.0,<2.0"

# Instalação a partir de arquivo requirements.txt
pip install -r requirements.txt

# Desinstalação
pip uninstall pacote

# Listar pacotes instalados
pip list

# Verificar info de pacote instalado
pip show pacote

# Criar requirements.txt
pip freeze > requirements.txt
```

### 1.2 Limitações

- Não gerencia ambientes virtuais
- Resolução de dependências simplista
- Falta de bloqueio de versões precisas para todas as dependências
- Difícil gerenciamento de dependências de desenvolvimento vs produção

---

## 2. Ambientes Virtuais

Ambientes virtuais são essenciais para isolar dependências de diferentes projetos.

### 2.1 venv (módulo padrão)

```bash
# Criando ambiente virtual
python -m venv meu_ambiente

# Ativando no Windows
meu_ambiente\Scripts\activate

# Ativando no Linux/MacOS
source meu_ambiente/bin/activate

# Desativando
deactivate
```

### 2.2 virtualenv (alternativa)

```bash
# Instalação
pip install virtualenv

# Criando ambiente virtual
virtualenv meu_ambiente

# Ativação (similar ao venv)
```

---

## 3. Pipenv - Unificando pip + virtualenv

Pipenv combina pip e virtualenv para fornecer uma solução mais robusta de gerenciamento de dependências.

### 3.1 Características Principais

- Arquivos `Pipfile` e `Pipfile.lock` substituem o requirements.txt
- Gerenciamento automático de ambientes virtuais
- Resolução de dependências avançada
- Separação entre dependências de desenvolvimento e produção

### 3.2 Comandos Básicos

```bash
# Instalação
pip install pipenv

# Instalar pacote (cria Pipfile e ambiente virtual automaticamente)
pipenv install requests

# Instalar dependência de desenvolvimento
pipenv install pytest --dev

# Instalar do Pipfile existente
pipenv install

# Instalar apenas dependências de produção
pipenv install --deploy

# Ativar ambiente
pipenv shell

# Executar comando no ambiente
pipenv run python script.py

# Gerar requirements.txt
pipenv requirements > requirements.txt
```

### 3.3 Exemplo de Pipfile

```toml
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = ">=2.25.0"
pandas = "*"

[dev-packages]
pytest = "*"
black = "*"

[requires]
python_version = "3.9"
```

---

## 4. Poetry - Gerenciamento Completo de Projetos

Poetry é uma ferramenta moderna para gerenciamento de dependências e empacotamento de projetos Python.

### 4.1 Características Principais

- Arquivos `pyproject.toml` e `poetry.lock`
- Resolução de dependências poderosa
- Versionamento semântico
- Publicação simplificada de pacotes
- Gerenciamento de ambientes virtuais

### 4.2 Comandos Básicos

```bash
# Instalação
pip install poetry

# Criar novo projeto
poetry new meu_projeto

# Inicializar em projeto existente
poetry init

# Adicionar dependência
poetry add requests

# Adicionar dependência de desenvolvimento
poetry add pytest --group dev

# Instalar dependências
poetry install

# Instalar só produção (sem dev)
poetry install --no-dev

# Ativar ambiente virtual
poetry shell

# Executar comando
poetry run python script.py

# Atualizar dependências
poetry update
```

### 4.3 Exemplo de pyproject.toml

```toml
[tool.poetry]
name = "meu-projeto"
version = "0.1.0"
description = "Descrição do projeto"
authors = ["Seu Nome <seu.email@exemplo.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.9"
requests = "^2.28.1"
pandas = "^1.5.0"

[tool.poetry.group.dev.dependencies]
pytest = "^7.1.3"
black = "^22.8.0"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

---

## 5. PDM - Python Development Master

PDM é uma nova alternativa que segue o padrão PEP 621 e utiliza o novo mecanismo de instalação do pip.

### 5.1 Características

- Compatível com PEP 621 (pyproject.toml)
- Instalação rápida (usa pip para instalar)
- Preserva a estrutura de dependências
- Suporte a scripts de projeto

### 5.2 Comandos Básicos

```bash
# Instalação
pip install pdm

# Inicializar projeto
pdm init

# Adicionar dependência
pdm add requests

# Adicionar dependência de desenvolvimento
pdm add pytest -d

# Instalar dependências
pdm install

# Executar script
pdm run python script.py
```

---

## 6. Boas Práticas

### 6.1 Gerais

- **SEMPRE use ambientes virtuais** - Nunca instale pacotes globalmente (exceto ferramentas como pip, pipenv, poetry)
- **Especifique versões** - Evite usar a última versão sem restrições
- **Use bloqueio de versões** - Arquivos como poetry.lock ou Pipfile.lock garantem reprodutibilidade
- **Separe dependências de dev e produção** - Mantenha ambientes limpos
- **Documente requisitos** - Informe Python mínimo e outras restrições
- **Fixe versões em produção** - Use hashes para máxima reprodutibilidade

### 6.2 requirements.txt Avançado

```
# Instalação com hash para segurança e reprodutibilidade
requests==2.28.1 --hash=sha256:7c5599b102feddaa661c826c56ab4fee28bfd17f5abbca...
```

### 6.3 Quando usar cada ferramenta?

- **pip + venv**: Projetos simples, scripts, ambiente didático
- **pipenv**: Projetos de médio porte, bom para equipes mistas
- **poetry**: Bibliotecas, projetos complexos, quando você quer empacotar e publicar
- **PDM**: Projetos novos que seguem os últimos padrões do Python

---

## 7. Comparação Rápida

| Característica            | pip              | pipenv       | poetry         | PDM            |
| ------------------------- | ---------------- | ------------ | -------------- | -------------- |
| Gerencia ambiente virtual | Não              | Sim          | Sim            | Sim            |
| Resolução de dependências | Básica           | Avançada     | Avançada       | Avançada       |
| Arquivo de configuração   | requirements.txt | Pipfile      | pyproject.toml | pyproject.toml |
| Arquivo de lock           | Não              | Pipfile.lock | poetry.lock    | pdm.lock       |
| Dependências de dev       | Manual           | Sim          | Sim            | Sim            |
| Publicação de pacotes     | Não              | Não          | Sim            | Sim            |
| Maturidade                | Alta             | Média        | Média          | Baixa          |

---

## 8. Referências

- [Pip Documentation](https://pip.pypa.io/en/stable/)
- [Pipenv Documentation](https://pipenv.pypa.io/en/latest/)
- [Poetry Documentation](https://python-poetry.org/docs/)
- [PDM Documentation](https://pdm.fming.dev/latest/)
- [Python Packaging User Guide](https://packaging.python.org/guides/)
