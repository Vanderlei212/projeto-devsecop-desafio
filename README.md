# Desafio DevSecOps — Gerenciador de Tarefas

## Sobre o Projeto

Este projeto faz parte do desafio prático do módulo de DevSecOps da ADA Tech.

O objetivo foi implementar uma pipeline de CI/CD com verificações de segurança, corrigir vulnerabilidades presentes no código e realizar o deploy da aplicação utilizando GitHub Pages.

## Pipeline DevSecOps

A pipeline está definida em:

`.github/workflows/pipeline.yml`

Ela é executada automaticamente a cada `push` na branch `main`.

### 1. Checkout do código

O GitHub Actions utiliza `actions/checkout` para baixar o código do repositório e disponibilizá-lo para as etapas seguintes.

### 2. Build

A etapa de Build verifica os arquivos da aplicação e confirma que a estrutura esperada está disponível.

### 3. Secrets Scanning — Gitleaks

O Gitleaks analisa o repositório em busca de informações sensíveis expostas, como chaves de API e senhas.

A pipeline deve falhar caso um segredo seja identificado.

### 4. SAST — Semgrep

O Semgrep realiza uma análise estática do código-fonte para identificar padrões que podem representar vulnerabilidades de segurança.

A execução utiliza:

`semgrep scan --config auto --error src/`

O parâmetro `--error` faz a etapa retornar erro quando forem encontrados problemas.

### 5. SCA — Grype

O Grype verifica as dependências e componentes utilizados pelo projeto em busca de vulnerabilidades conhecidas.

A execução utiliza:

`grype dir:. --fail-on medium`

Dessa forma, vulnerabilidades de severidade média ou superior fazem a pipeline falhar.

### 6. Deploy — GitHub Pages

O deploy só é executado depois que as etapas anteriores terminam com sucesso.

A aplicação é publicada utilizando:

- `actions/configure-pages`
- `actions/upload-pages-artifact`
- `actions/deploy-pages`

## Vulnerabilidades Corrigidas

O projeto original continha vulnerabilidades propositalmente inseridas para o exercício.

Entre os problemas identificados estavam:

- Chave de API exposta diretamente no código.
- Senha de banco de dados exposta diretamente no código.
- Uso de `innerHTML` com entrada fornecida pelo usuário.
- Uso de `eval()` com entrada fornecida pelo usuário.

Esses problemas foram corrigidos antes do deploy final.

O código atual utiliza criação de elementos DOM e `innerText` para inserir tarefas, evitando a execução de conteúdo fornecido pelo usuário.

## Resultado da Pipeline

A pipeline final foi executada com sucesso no GitHub Actions.

As verificações de segurança foram concluídas sem vulnerabilidades bloqueantes e o deploy foi realizado no GitHub Pages.

## URL de Produção

https://vanderlei212.github.io/projeto-devsecop-desafio/

## Tecnologias Utilizadas

- Git
- GitHub
- GitHub Actions
- Gitleaks
- Semgrep
- Grype
- GitHub Pages
- JavaScript
- Node.js
