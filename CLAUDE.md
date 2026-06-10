# Working in this repo

Project context for Claude Code. Loaded into every conversation.

## What this repo is

A web app being built from the `web-starter` template. Stack: **Next.js 16 + Auth.js v5 + Postgres + Drizzle + Sentry**. Deploy: Vercel (frontend) + Render (backend / DB).

## Discipline plugin (required)

```text
/plugin marketplace add Lizo-RoadTown/claude-skills-marketplace
/plugin install make-skills-discipline@lizo-skills
```

PROBE before asserting (cite file:line). Distinguish dev-tooling from runtime (`platform/api/` + `app/` = runtime; `scripts/` + `docs/` = dev-tooling). Save corrections as feedback memory.

## Stack reference

| Layer | Tech | Notes |
|---|---|---|
| Frontend | Next.js 16 (App Router), React 19, Tailwind v4 | |
| Auth | Auth.js v5, HS256 JWT | If integrating with Make_Skills engine, share `AUTH_SECRET` |
| DB | Postgres + Drizzle | Drizzle is the query layer; if a Python backend exists, its migrations are source of truth |
| Errors | `@sentry/nextjs ^10.x` | Next 16 needs Sentry 10; the v8.x line is incompatible |
| Deploy | Vercel + Render | |

## Skills you can lean on

- `skills/agentic-skill-design`, `deep-research-pattern`, `design-evaluation`, `documentation`, `document-parsing`, `eval-deep-research`, `infrastructure-mapping`, `layered-explanation`, `next-actions-planning`
- `skills_private/agentic-upskilling`, `lessons-learned`, `proposal-authoring`, `orchestration-cataloging`, `roadmap-maintenance`, `web-app-scaffold`, `open-source-documentation`

## Memory hierarchy

1. **`~/.claude/projects/<this-repo-key>/memory/MEMORY.md`** — auto-loaded session memory
2. **the-loom MCP** at `https://loom-agent-context.onrender.com/mcp/memory/` — tag memories with this project's `LOOM_PROJECT_ID`
3. **`docs/proposals/*.md`** — architectural decisions
4. **`docs/plans/*.md`** — `YYYY-MM-DD-name.md`
5. **`docs/test-runs/*.md`** — friction-surface logs
6. **Git history** — explain *why*

## Tone

No marketing voice. Describe what *is*. Plain, direct, descriptive.

## Commit + PR discipline

- Small PRs, one concern per branch
- `gh pr create` with Test Plan checklist
- Never `--no-verify`, never `--amend` after pushing
- Co-author tag: `Co-Authored-By: Claude Opus 4.7 (1M context) <noreply@anthropic.com>`

## What to do when in doubt

1. Check loaded memory + the-loom MCP (`memory_recall`)
2. Read the relevant skill
3. Scope code reads tight: Grep first, Read with offset/limit
4. If about to make a destructive change, ask first
