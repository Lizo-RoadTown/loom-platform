# Loom Platform

The official site for **the loom** — a multi-module platform that helps any agent-using project produce, observe, and durably store the structure it generates.

This site is itself a consumer of the platform it describes. The architecture demonstrates itself by being itself.

## What the loom platform is

The loom is a set of cooperating modules. Each has its own concern:

| Module | Role |
|---|---|
| **[Make_Skills](https://github.com/Lizo-RoadTown/Make_Skills)** | The recursive skill engine. Per-turn agent loop, skill compilation, model registry, subagent orchestration, methodology skills library. |
| **the-loom** | The platform substrate. Memory MCP (cross-project semantic memory), Project Registry, Project Observatory, Architecture Registry, promotion governance, durable structure. |
| **Consumer applications** | Each consumer is its own repo + its own `.project-intelligence/<instance-id>/`. Different consumer shapes (classroom, development, research) attach a Layer-2 adapter from Make_Skills. This site is one. |

The anchor boundary rule: *Make_Skills improves local agency and produces candidates. The-loom observes across projects, governs promotion, and stores durable structure.*

## What this repo is

`loom-platform` is the public-facing site for the loom set. Its job:

1. **Show the architecture working.** Live demos backed by real module wiring — not screenshots of slides.
2. **Document how to use the modules together.** Setup steps, integration patterns, what each module owns.
3. **Be a reference consumer.** Anyone wanting to build a loom-aware app can read this repo's code to see how `.project-intelligence/`, the Project Registry handshake, the discipline plugin, and the methodology skills compose.

## Stack

- **Next.js 16** (App Router, React 19, Tailwind v4)
- **Auth.js v5** (HS256 JWT)
- **Postgres** (Render for hosted, Docker for local)
- **Drizzle ORM**
- **Sentry** (`@sentry/nextjs ^10.x`)
- **Vercel** for the frontend; **Render** for any backend services

## What ships in this seed

- `CLAUDE.md` — agent context for working in this repo (CORE DIRECTIVE 1: loom-memory access mandatory)
- `skills/` + `skills_private/` — the 16-skill methodology library from Make_Skills, bundled locally per the loom seed pattern
- `.env.template` — env vars the runtime + telemetry needs
- `LICENSE` — Apache 2.0

The site code (Next.js app + content) gets built on top of this seed in subsequent commits.

## Setup

```powershell
# 1. Clone
git clone https://github.com/Lizo-RoadTown/loom-platform.git
cd loom-platform

# 2. Configure env
cp .env.template .env
# Edit .env with values from your-machine's the-loom/.env (OTel endpoint + headers)
# Generate AUTH_SECRET: openssl rand -base64 32

# 3. Initialize Next.js (first-time only, when ready to build the site)
npx create-next-app@latest . --typescript --tailwind --app --no-src-dir --import-alias "@/*"
npm install next-auth @auth/drizzle-adapter drizzle-orm pg @sentry/nextjs
npm install -D drizzle-kit @types/pg

# 4. Run
npm run dev
```

## Loom-platform-specific integration

In addition to the standard loom onboarding (`.env` + Project Registry registration), this site will:

- Read from the **loom-memory MCP** to surface cross-project state — what's been promoted, what's in candidate review, recent activity. (Eventually; v1 of the site can be static.)
- Read from the **Project Registry** to enumerate registered consumer projects (with public/private filtering).
- Subscribe to **Project Observatory** telemetry for the public dashboard view.

These come online progressively as the site gets built.

## Status

- **2026-06-10:** Seed shape committed. Site code TBD.
- The repo is **public from day one** because this site IS the public face of the loom.

## Related

- [Make_Skills](https://github.com/Lizo-RoadTown/Make_Skills) — the engine module (public)
- [project-starter](https://github.com/Lizo-RoadTown/project-starter) — day-1 Claude Code scaffolding (public)
- [claude-skills-marketplace](https://github.com/Lizo-RoadTown/claude-skills-marketplace) — public discipline plugin + skills marketplace

## License

Apache 2.0 — see [LICENSE](LICENSE).
