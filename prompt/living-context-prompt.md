# Living Context — Prompt Reutilizável para Qualquer IA

> **O que é isso?** Um prompt que você pode colar em qualquer assistente de IA (ChatGPT, Gemini, Copilot, Claude, etc.) para criar e manter um "context vault" — um sistema de documentação vivo que evolui junto com o código do seu projeto.
>
> **Como usar:** Copie o prompt abaixo e cole na conversa com a IA. Substitua `{INFORMAÇÕES DO SEU PROJETO}` pelas informações reais. Depois, a cada nova conversa de desenvolvimento, cole apenas a **Seção B** como lembrete de manutenção.

---

## Seção A — Criação Inicial do Context Vault

Cole este prompt para a IA criar o vault do zero:

```
Preciso que você crie um "Living Context Vault" para o meu projeto. É um sistema de documentação em Markdown (formato Obsidian) que evolui junto com o código — não é documentação estática, é um vault vivo que será atualizado a cada mudança significativa no código.

## Informações do Projeto

{PREENCHA ABAIXO — apague os exemplos e coloque os dados reais do seu projeto}

- **Nome do projeto**: {Ex: Meu App}
- **Tipo de projeto**: {Ex: API REST, App mobile, SaaS fullstack, Data pipeline, CLI tool, Monorepo...}
- **Stack/tecnologias**: {Ex: Python + FastAPI + PostgreSQL, React + Node, Flutter + Firebase...}
- **Áreas/domínios do projeto**: {Ex: autenticação, pagamentos, relatórios, pipeline de dados, etc.}
- **Idioma da documentação**: {português / inglês / misto}
- **Idioma do código**: {inglês / misto}
- **Estado atual**: {projeto novo / em andamento com X features / legado sendo reescrito}
- **Informações extras relevantes**: {time, deadline, contexto, o que quiser}

## O que você deve criar

### 1. Estrutura de pastas `context/`

Crie uma pasta `context/` na raiz do projeto com:

**Arquivos obrigatórios:**
- `HOME.md` — Página inicial com visão geral, navegação por wikilinks `[[]]`, tabela de stack, e um callout `> [!important]` explicando que o vault é vivo
- `changelog.md` — Log de todas as mudanças significativas, no formato:
  ```
  ## [YYYY-MM-DD] Tipo: Descrição curta
  - **Arquivos afetados**: lista
  - **O que mudou**: descrição do que foi alterado e por quê
  ```
  Tipos: Feature | Bugfix | Refactor | UI/UX | Infraestrutura | Documentação

**Pastas de domínio (adapte ao meu projeto):**
Crie pastas numeradas (01-xxx, 02-xxx...) para cada área principal do projeto. Cada pasta deve ter pelo menos:
- `estrutura.md` — Layout de pastas, convenções, stack, comandos
- `evolucao.md` — Histórico de mudanças deste domínio específico, com referências a decisões via wikilinks
- `decisoes.md` — ADRs (Architecture Decision Records) numerados (AD-001, AD-002...) com: Data, Contexto, Decisão, Consequência

**Tracking de features:**
- Uma pasta com `features.md` — Checklist de features implementadas e planejadas

**Arquivos de domínio adicionais** (conforme o projeto precise):
- `modelos-dados.md` — ERD em Mermaid, descrição de entidades
- `regras-negocio.md` — Regras de domínio
- `auth.md` — Fluxo de autenticação e permissões
- `api.md` — Endpoints e contratos
- `design-system.md` — Tokens de design, componentes base
- `rotas.md` — Rotas/navegação do app
- Qualquer outro arquivo que faça sentido para o projeto

### 2. Formato dos arquivos

Todos os arquivos devem usar:

**Frontmatter YAML:**
```yaml
---
title: Título da Página
tags:
  - tag-relevante
  - dominio
aliases:
  - Nome Alternativo
---
```

**Wikilinks para navegação:**
- Entre arquivos: `[[pasta/arquivo|Label]]`
- Para seções: `[[arquivo#seção]]`
- Para ADRs: `[[decisoes#AD-001]]`

**Callouts Obsidian:**
- `> [!important]` — Informações críticas
- `> [!tip]` — Convenções e boas práticas
- `> [!warning]` — Cuidados
- `> [!info]` — Formatos e estruturas
- `> [!example]` — Exemplos
- `> [!todo]` — Itens pendentes

**Mermaid para diagramas** quando útil (ERD, fluxos, sequências)

### 3. Regras de manutenção no CLAUDE.md (ou no arquivo de instruções da IA)

Crie ou atualize o arquivo de instruções da IA (`CLAUDE.md`, `.cursorrules`, `AGENTS.md`, ou equivalente) na raiz do projeto com a seguinte seção:

```
## Manutenção do Context Vault

Após QUALQUER modificação significativa de código, OBRIGATORIAMENTE:

1. Atualize `context/changelog.md` com:
   - Data: `[YYYY-MM-DD]`
   - Tipo: Feature | Bugfix | Refactor | UI/UX | Infraestrutura | Documentação
   - Arquivos afetados
   - O que mudou e por quê

2. Atualize o arquivo de evolução do domínio afetado (ex: `context/01-xxx/evolucao.md`)

3. Se uma decisão arquitetural foi tomada, adicione novo ADR no `decisoes.md` relevante

4. Se modelos de dados mudaram, atualize o arquivo de modelos de dados

5. Se uma feature foi adicionada ou completada, atualize o tracking de features

O vault `context/` é vivo — crie, renomeie, divida ou remova arquivos conforme necessário.
Use formato Obsidian: frontmatter YAML, wikilinks `[[]]`, callouts `> [!tipo]`.
```

### 4. Conteúdo inicial

Preencha os arquivos com o estado ATUAL do projeto:
- Documente a arquitetura existente
- Registre decisões já tomadas (mesmo que retroativamente)
- Preencha o checklist de features com o status atual
- Crie a primeira entrada no changelog: `[{data de hoje}] Infraestrutura: Context vault inicializado`

## Importante

- Adapte a estrutura ao MEU projeto — não use uma estrutura genérica que não faz sentido
- Os nomes das pastas e arquivos devem refletir os domínios reais do meu projeto
- Se o projeto tiver áreas que não listei, crie arquivos para elas
- Se alguma área que listei não fizer sentido, não crie
- Use o idioma que eu especifiquei para a documentação
```

---

## Seção B — Lembrete de Manutenção (para conversas subsequentes)

Cole este lembrete no início de conversas onde você vai modificar código:

```
IMPORTANTE: Este projeto possui um Living Context Vault na pasta `context/`.

Antes de começar, leia `context/HOME.md` para entender o projeto.

Após QUALQUER modificação significativa de código, você DEVE:
1. Atualizar `context/changelog.md` (data, tipo, arquivos, o que mudou)
2. Atualizar o `evolucao.md` do domínio afetado
3. Adicionar ADR em `decisoes.md` se decisão arquitetural foi tomada
4. Atualizar `modelos-dados.md` se schema mudou
5. Atualizar `features.md` se feature foi adicionada/completada

O vault é vivo — crie novos arquivos se necessário. Use formato Obsidian (frontmatter, wikilinks, callouts).
```

---

## Seção C — Prompt para Auditar/Atualizar um Vault Existente

Use quando o vault ficou desatualizado:

```
Meu projeto tem um Living Context Vault na pasta `context/` mas ele pode estar desatualizado.

Por favor:
1. Leia `context/HOME.md` e todos os arquivos do vault
2. Compare com o estado atual do código (explore a codebase)
3. Identifique o que está faltando, desatualizado, ou incorreto
4. Atualize tudo que precisa ser atualizado
5. Adicione entradas no changelog para as mudanças que você fez
6. Me diga o que encontrou e o que atualizou
```

---

## Dicas de Uso

### Qual arquivo de instruções usar?

| Ferramenta | Arquivo |
|-----------|---------|
| Claude Code | `CLAUDE.md` |
| Cursor | `.cursorrules` |
| GitHub Copilot | `.github/copilot-instructions.md` |
| Windsurf | `.windsurfrules` |
| Aider | `.aider.conf.yml` + conventions |
| Gemini CLI | `GEMINI.md` |
| Qualquer IA (chat) | Cole a Seção B no início da conversa |

### Quando atualizar o vault?

- **Sempre:** após adicionar features, corrigir bugs, refatorar, ou mudar arquitetura
- **Opcional:** para mudanças triviais (fix typo, ajuste de estilo)
- **Periodicamente:** use a Seção C para auditar e atualizar o vault

### Como abrir o vault no Obsidian?

1. Abra o Obsidian
2. "Open folder as vault" → selecione a pasta `context/`
3. Use o Graph View para visualizar as conexões entre documentos
