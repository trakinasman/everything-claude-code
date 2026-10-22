# Seleção do ECC (everything-claude-code)

Peças escolhidas do [affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)
(tag `v2.2.3`, commit `c05b2d6`, licença MIT — ver `LICENSE`) para a nossa stack:
React/Vite, Node/Express, Python/FastAPI + ETL, Postgres (Cloud SQL), Docker/Cloud Run.

Tudo aqui é Markdown puro: nenhum script, hook ou chamada de rede. Os arquivos estão como no
original, sem edição.

**Esta pasta só guarda a seleção.** O Claude Code não carrega nada daqui sozinho; veja
"Instalar" abaixo.

## O que entrou

### Agentes (`agents/`) — chamados sob demanda para revisar ou corrigir

| Agente | Para quê |
|---|---|
| `code-reviewer` | revisão geral de código |
| `security-reviewer` | segurança (segredos, injeção, auth) |
| `react-reviewer` | componentes React, hooks, acessibilidade |
| `typescript-reviewer` | TSX/TS (ex.: `noc_frontend/src/pages`) |
| `python-reviewer` | Python em geral (ETLs) |
| `fastapi-reviewer` | `noc_v8_api` |
| `database-reviewer` | SQL, índices, migrações |
| `silent-failure-hunter` | erros engolidos (`except: pass`, `catch {}`), útil nos ETLs |
| `build-error-resolver` | erros de build/tipos (este edita arquivos) |

Todos declaram `model: sonnet` e usam Read/Grep/Glob/Bash (o `build-error-resolver` também
Write/Edit).

### Skills (`skills/`) — carregadas quando o assunto aparece

`react-patterns`, `vite-patterns`, `python-patterns`, `fastapi-patterns`, `postgres-patterns`,
`database-migrations`, `docker-patterns`, `deployment-patterns`, `error-handling`,
`security-review`.

Observações: `database-migrations` traz exemplos de Prisma/Drizzle (não usamos, mas os princípios
valem); `docker-patterns` tem uma seção final sobre testar o próprio ECC em Docker, que pode ser
ignorada.

## O que ficou de fora, e por quê

- **Hooks**: rodam sozinhos a cada ação (formatador, bloqueios, gravação de sessão). Risco de
  efeito colateral alto e o Claude Code já tem memória própria.
- **Regras (`rules/`)**: são carregadas sempre e impõem TDD obrigatório com 80% de cobertura,
  "sempre usar subagentes em paralelo" etc. Conflitam com projetos sem testes como o `crm-q13`.
  Se quiser algo daí, a candidata é `rules/common/security.md`.
- **Comandos (`commands/`)**: a maioria duplica recursos nativos (`/plan`, `/code-review`,
  `/security-review`, `/simplify`) ou depende da infraestrutura do ECC.
- **Skills e agentes de outras stacks**: Go, Rust, Java, Kotlin, Swift, .NET, Django, Laravel,
  Flutter, Vue/Angular, marketing, finanças, saúde, homelab etc.
- **Sobreposição com nativos**: `planner`, `code-simplifier`, `code-explorer`, `docs-lookup`.

## Instalar (quando decidir)

Copie só o que quiser usar:

- **Para um projeto** (ex.: `crm-q13`): para `<projeto>/.claude/agents/` e
  `<projeto>/.claude/skills/` — vale para quem abrir o Claude Code naquele projeto.
- **Para todos os seus projetos**: para `~/.claude/agents/` e `~/.claude/skills/`.

Depois, reinicie a sessão do Claude Code para ele carregar.

**Atenção ao instalar:** a skill `security-review` tem o mesmo nome do comando nativo
`/security-review`. Instale-a como `security-checklist` (pasta e campo `name:` do `SKILL.md`)
para não sobrepor o nativo. Foi assim que ela entrou em `~/.claude/skills/` em 2026-10-21,
junto com os 9 agentes e as outras 9 skills.

## Atualizar a partir do original

Compare com a tag nova do upstream e copie de novo só as mesmas pastas listadas acima.
