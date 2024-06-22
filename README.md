# Deploy-Automatizado-de-um-Blog-Utilizando-GitHub-Actions-e-AWS

Automatizar o processo de deploy de um blog utilizando GitHub Actions e AWS envolve integrar as capacidades de automação oferecidas pelo GitHub com os serviços de computação em nuvem robustos da AWS. Vamos detalhar como configurar um fluxo automatizado que permite atualizar e implantar um site estático hospedado na AWS S3 utilizando GitHub Actions como ferramenta de CI/CD.

### Configuração Inicial do Projeto

1. **Setup do Projeto:**
   - Configure um repositório Git para o seu projeto de blog no GitHub.
   - Garanta que o AWS CLI esteja configurado localmente para interação com serviços da AWS.

2. **Estrutura do Projeto:**
   Organize o projeto em módulos e arquivos necessários para o blog estático.
   ```
   /meu-blog
   ├── /src
   │   ├── index.html
   │   ├── /css
   │   │   ├── styles.css
   │   ├── /js
   │   │   ├── script.js
   ├── /scripts
   │   ├── deploy.sh
   ├── /.github
   │   ├── workflows
   │   │   ├── deploy.yml
   ├── README.md
   ```

### Configuração do GitHub Actions

1. **Criar Arquivo de Fluxo (Workflow):**
   - Configure um arquivo YAML para o GitHub Actions que automatiza o deploy do blog.

   Exemplo de arquivo (`deploy.yml`):
   ```yaml
   name: Deploy to AWS S3

   on:
     push:
       branches:
         - main

   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout code
           uses: actions/checkout@v2

         - name: Install AWS CLI
           run: |
             sudo apt-get update
             sudo apt-get install -y awscli

         - name: Deploy to S3
           run: |
             aws s3 sync ./src s3://meu-blog-bucket --delete
   ```

   - **Explicação:**
     - O workflow é acionado no push para a branch `main`.
     - Ele roda em uma máquina virtual `ubuntu-latest`.
     - `Checkout code` faz o download do código do repositório.
     - `Install AWS CLI` instala o AWS CLI para interação com a AWS.
     - `Deploy to S3` sincroniza o conteúdo da pasta `src` local com o bucket S3 configurado para o blog.

### Configuração da AWS S3

1. **Configurar Bucket S3:**
   - Crie um bucket S3 na AWS para armazenar o conteúdo estático do blog.

   Exemplo de comando AWS CLI para criação do bucket:
   ```bash
   aws s3 mb s3://meu-blog-bucket --region us-east-1
   ```

2. **Políticas de Acesso:**
   - Configure políticas de acesso no bucket S3 para permitir acesso público ao conteúdo do blog, se necessário.

### Teste e Integração

1. **Testar o Deploy Automatizado:**
   - Faça um push para a branch `main` no repositório do GitHub para acionar o workflow.
   - Verifique o console do GitHub Actions para monitorar o progresso do deploy.

2. **Acesso ao Blog:**
   - Após o deploy, verifique se o blog está acessível via URL pública do bucket S3.

### Conclusão

Automatizar o deploy de um blog utilizando GitHub Actions e AWS S3 simplifica o processo de atualização e distribuição de conteúdo estático na web. Este projeto não apenas demonstra como configurar um fluxo de CI/CD eficiente com GitHub Actions, mas também aproveita a infraestrutura escalável e confiável da AWS para hospedar o blog de forma segura e acessível. A integração dessas tecnologias permite um desenvolvimento ágil e seguro, alavancando o poder da nuvem para otimizar o ciclo de vida do desenvolvimento de software.
