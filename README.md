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

## 🔧 Como Adicionar Novos Workflows

1. Crie o arquivo em `.github/workflows/`
2. Use `workflow_call` como trigger
3. Documente os inputs neste README
4. Faça push na branch `main`

## ⚠️ Requisitos

- O repositório precisa ser **público** ou estar com o compartilhamento de workflows habilitado nas [configurações da organização](https://docs.github.com/en/organizations/managing-organization-settings/sharing-workflows-with-your-organization).
