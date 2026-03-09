# Kairogen Workflows

Repositório centralizado de **workflows reutilizáveis** do GitHub Actions para todas as aplicações da Kairogen.

## 📦 Workflows Disponíveis

### [Code Check](.github/workflows/code-check.yml)

Pipeline de qualidade de código com etapas encadeadas: **Lint → Test → Build**.

```yaml
jobs:
  code-check:
    uses: kairogenai/workflows/.github/workflows/code-check.yml@main
```

#### Inputs

| Input | Tipo | Default | Descrição |
|---|---|---|---|
| `node-version` | `string` | `22` | Versão do Node.js |
| `pnpm-version` | `number` | `9` | Versão do pnpm |
| `run-lint` | `boolean` | `true` | Executar lint |
| `run-test` | `boolean` | `true` | Executar testes |
| `run-build` | `boolean` | `true` | Executar build |

#### Exemplos

**Executar tudo (padrão):**

```yaml
name: CI

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main, develop]

jobs:
  code-check:
    uses: kairogenai/workflows/.github/workflows/code-check.yml@main
```

**Apenas lint e test (sem build):**

```yaml
jobs:
  code-check:
    uses: kairogenai/workflows/.github/workflows/code-check.yml@main
    with:
      run-build: false
```

**Com versão customizada do Node:**

```yaml
jobs:
  code-check:
    uses: kairogenai/workflows/.github/workflows/code-check.yml@main
    with:
      node-version: '20'
```

### [Code Check Front](.github/workflows/code-check-front.yml)

Pipeline simplificada para Frontend (apenas **Build**).

```yaml
jobs:
  code-check:
    uses: kairogenai/workflows/.github/workflows/code-check-front.yml@main
```

#### Inputs

| Input | Tipo | Default | Descrição |
|---|---|---|---|
| `node-version` | `string` | `"22"` | Versão do Node.js |
| `pnpm-version` | `number` | `9` | Versão do pnpm |
| `run-build` | `boolean` | `true` | Executar build |

---

### [Build and Push to ECR](.github/workflows/build-image.yml)

Gera imagem Docker e faz o push para o Amazon ECR.

```yaml
jobs:
  build:
    uses: kairogenai/workflows/.github/workflows/build-image.yml@main
    with:
      repository: 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-repo
      image_tag: ${{ github.sha }}
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

#### Inputs

| Input | Tipo | Default | Descrição |
|---|---|---|---|
| `repository` | `string` | **Obrigatório** | URI do repositório ECR |
| `image_tag` | `string` | **Obrigatório** | Tag da imagem |
| `environment` | `string` | `"production"` | Ambiente (afeta a região AWS: production=us-east-1, outros=us-east-2) |

#### Secrets

| Secret | Obrigatório | Descrição |
|---|---|---|
| `AWS_ACCESS_KEY_ID` | Sim | ID da chave de acesso AWS |
| `AWS_SECRET_ACCESS_KEY` | Sim | Chave de acesso secreta AWS |

---

### [Update Cluster Containers](.github/workflows/update-cluster-containers.yml)

Atualiza todos os serviços de um cluster ECS que utilizam uma imagem específica.

```yaml
jobs:
  deploy:
    uses: kairogenai/workflows/.github/workflows/update-cluster-containers.yml@main
    with:
      repository: 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-repo
      image_tag: v1.0.0
      cluster: my-cluster
    secrets:
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```

#### Inputs

| Input | Tipo | Default | Descrição |
|---|---|---|---|
| `repository` | `string` | **Obrigatório** | URI do repositório da imagem (sem tag) |
| `image_tag` | `string` | **Obrigatório** | Nova tag da imagem |
| `cluster` | `string` | **Obrigatório** | Nome do cluster ECS |
| `aws_region` | `string` | `"us-east-1"` | Região AWS |

#### Secrets

| Secret | Obrigatório | Descrição |
|---|---|---|
| `AWS_ACCESS_KEY_ID` | Sim | ID da chave de acesso AWS |
| `AWS_SECRET_ACCESS_KEY` | Sim | Chave de acesso secreta AWS |

## 🔧 Como Adicionar Novos Workflows

1. Crie o arquivo em `.github/workflows/`
2. Use `workflow_call` como trigger
3. Documente os inputs neste README
4. Faça push na branch `main`

## ⚠️ Requisitos

- O repositório precisa ser **público** ou estar com o compartilhamento de workflows habilitado nas [configurações da organização](https://docs.github.com/en/organizations/managing-organization-settings/sharing-workflows-with-your-organization).
