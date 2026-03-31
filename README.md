# Living Context Plugin

Um plugin para Claude Code que cria e mantém um **context vault vivo** em qualquer projeto — um sistema de documentação em Obsidian Markdown que evolui junto com o código.

## O que faz?

- Analisa seu projeto (qualquer tipo: API, frontend, mobile, data science, infra, monorepo...)
- Cria uma pasta `context/` com documentação estruturada adaptada aos domínios do projeto
- Configura regras no `CLAUDE.md` para que a IA **obrigatoriamente** atualize o vault após cada mudança
- Usa formato Obsidian (frontmatter YAML, wikilinks `[[]]`, callouts `> [!type]`, Mermaid)

## Estrutura gerada

```
context/
├── HOME.md                    # Página inicial com navegação
├── changelog.md               # Log de todas as mudanças
├── 01-{dominio}/              # Pastas por domínio (adaptadas ao projeto)
│   ├── estrutura.md           # Layout, convenções, stack
│   ├── evolucao.md            # Histórico de mudanças
│   └── decisoes.md            # ADRs (Architecture Decision Records)
├── XX-features/
│   └── features.md            # Checklist de features
└── .obsidian/                 # Config do Obsidian (opcional)
```

## Instalação

### Opção 1: Clone direto

```bash
git clone https://github.com/SEU-ORG/living-context-plugin.git
```

Depois no Claude Code:
```
/plugin install ./living-context-plugin
```

### Opção 2: Copia manual

Copie a pasta para `~/.claude/skills/living-context/` e reinicie o Claude Code.

## Como usar

No Claude Code, diga qualquer coisa como:

- `crie um living context para este projeto`
- `configure o context vault`
- `inicialize a documentação viva deste projeto`

A skill será ativada automaticamente.

## Para quem NÃO usa Claude Code

O arquivo `prompt/living-context-prompt.md` contém um **prompt reutilizável** que funciona em qualquer IA:

| Ferramenta | Como usar |
|-----------|----------|
| ChatGPT / Gemini / Claude (chat) | Cole a **Seção A** do prompt |
| Cursor | Copie as regras de manutenção para `.cursorrules` |
| GitHub Copilot | Copie para `.github/copilot-instructions.md` |
| Windsurf | Copie para `.windsurfrules` |
| Gemini CLI | Copie para `GEMINI.md` |

## O segredo: manutenção obrigatória

A documentação só funciona se for atualizada. O plugin adiciona regras ao `CLAUDE.md` que **forçam** a IA a atualizar o vault após cada mudança significativa de código. Sem isso, qualquer documentação morre em semanas.

## Licença

MIT
