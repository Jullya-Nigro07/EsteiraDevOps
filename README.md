DevOps: Pipeline CI/CD com GitHub Actions e Vercel

---

## 🤖 Visão Geral

O projeto tem como objetivo principal implementar um **pipeline de Integração Contínua (CI) e Entrega Contínua (CD)** automatizado, utilizando o **GitHub Actions** para gerenciar a construção, teste e implantação de uma API. A aplicação é conteinerizada com **Docker** e o deploy final é realizado na plataforma **Vercel**.

---

## 🚀 Tecnologias Utilizadas

* **Linguagem:** Python
* **Conteinerização:** Docker
* **Testes:** `unittest`
* **CI/CD:** GitHub Actions
* **Plataforma de Deploy:** Vercel

---

## 📋 Pré-requisitos

Para replicar este projeto localmente, você precisará ter instalado:

* **Git**
* **Docker**
* **Python 3.x**

---
## ⚙️ Configuração e Execução

### 1. Clonar o Repositório

```bash
git clone https://github.com/Impacta-Jullya-Nigro/Lab-Devops.git
cd Lab-Devops
```

---

## 2. Construir a Imagem Docker

Na raiz do projeto (onde o Dockerfile está localizado), execute:

```bash
docker build -t api-devops .
```

---

### 3. Executar os Testes no Container Docker

Para garantir que os testes unitários (test_app.py) estejam passando antes de fazer o *push*, você pode executá-los diretamente em um container da imagem recém-construída. A flag `--rm` garante que o container seja excluído após a execução.

```bash
docker run --rm api-devops python -m unittest test_app -v
```

---

## 🌐 Pipeline de CI/CD (GitHub Actions)

O pipeline de CI/CD é definido no arquivo:

 - .github/workflows/pipeline.yml

Ele é acionado a cada push para o branch **main**.

---

### 📌 Estrutura do Pipeline

#### **build** (executado no push)
- Instala as dependências do Python (`pip install -r requirements.txt`)
- Constrói a imagem Docker da aplicação (`api-devops`)

#### **test** *(needs: build)*
- Executa os testes unitários dentro do container Docker criado no build

#### **deploy** *(needs: test)*
- Se os testes forem bem-sucedidos, realiza a implantação na Vercel

---

### 🔐 Variáveis Secretas para Deploy

Para que o *job* de deploy funcione, é necessário configurar as seguintes **Secrets** no seu repositório do GitHub  
(Settings → Secrets and variables → Actions):

- `VERCEL_TOKEN`
- `VERCEL_ORG_ID`
- `VERCEL_PROJECT_ID`

