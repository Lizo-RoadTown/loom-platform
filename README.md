# web-starter

Template for a **Next.js + Auth.js + Postgres web app**. Wired for the-loom's onboarding flow (`.env` + Project Registry registration).

For framework-agnostic apps (desktop, mobile, CLI), use [`ux-starter`](https://github.com/Lizo-RoadTown/ux-starter) instead.

## What this template gives you

- **Skills library** (`skills/` + `skills_private/`) — 16 methodology skills copied from Make_Skills
- **Stack pre-decided**: Next.js (App Router) + Auth.js + Postgres + Drizzle. Add other deps as you build.
- **Docs scaffolding** — `docs/proposals/`, `docs/plans/`, `docs/test-runs/`
- **Agents placeholder** — `agents/` for your specific agent persona
- **Onboarding flow** — `.env.template` + Project Registry registration step (see [the-loom onboarding doc](https://github.com/Lizo-RoadTown/the-loom/blob/main/docs/howto/onboard-a-project.md))

## Recommended stack

- **Next.js 16** (App Router, React 19, Tailwind v4)
- **Auth.js v5** (HS256 JWT, `AUTH_SECRET` shared if integrating with the Make_Skills engine)
- **Postgres** (Render for hosted, Docker for local)
- **Drizzle ORM** (query layer; Python migration as source of truth if you also have a Python backend)
- **Sentry** (`@sentry/nextjs ^10.x` for Next 16 compat)
- **Deploy**: Vercel for the frontend; Render for any backend services

This mirrors the [`humancensys-app`](https://github.com/Lizo-RoadTown/humancensys-app) stack (which is also a `web-starter`-style consumer of the Make_Skills engine).

## Setup — quick path (scaffolder)

If you've cloned `the-loom` locally and merged PR #1, scaffold a new project in one command:

```powershell
cd C:\Users\Liz\the-loom
.\scripts\new-loom-project.ps1 -Template web -Name your-project-slug -DisplayName "Your Project Name"
```

This automates the loom-side onboarding steps. You still need to do the web-stack-specific setup (steps 7-8 below).

## Setup — manual path (8 steps, ~15 min)

Mirrors `the-loom/docs/howto/onboard-a-project.md` plus the web-stack steps. Do this if the scaffolder isn't available.

### Step 1 — Create the repo

```powershell
gh repo create Lizo-RoadTown/<project-slug> --private --clone
cd <project-slug>
```

Pick the slug carefully — it becomes `LOOM_PROJECT_ID`, the Grafana label, and the Project Registry's primary lookup key.

### Step 2 — Register the project in the-loom's Project Registry

Requires a JWT token (mint via `python the-loom/scripts/mint_loom_token.py --hours 720`).

```powershell
$token = "<paste-jwt-token>"
curl -X POST https://loom-project-registry.onrender.com/projects `
  -H "Authorization: Bearer $token" `
  -H "Content-Type: application/json" `
  -d '{\"slug\": \"<project-slug>\", \"name\": \"<Human-readable name>\", \"description\": \"<one sentence>\"}'
```

Idempotent check before creating:

```powershell
curl -H "Authorization: Bearer $token" https://loom-project-registry.onrender.com/projects/by-slug/<project-slug>
```

### Step 3 — Create the project's `.env` file

Copy `.env.template` to `.env` and fill in:

- `LOOM_PROJECT_ID` — the slug you chose in step 1
- `OTEL_EXPORTER_OTLP_ENDPOINT` — copy from the-loom's `.env`
- `OTEL_EXPORTER_OTLP_HEADERS` — copy from the-loom's `.env`
- `DATABASE_URL` — your Postgres connection string
- `AUTH_SECRET` — generate with `openssl rand -base64 32`
- `NEXTAUTH_URL` — your local or deployed URL

### Step 4 — Install the discipline plugin (if not already installed)

```text
/plugin marketplace add Lizo-RoadTown/claude-skills-marketplace
/plugin install make-skills-discipline@lizo-skills
```

Restart Claude Code — plugin loader binds at session start.

### Step 5 — Verify telemetry flows

Send a test prompt in Claude Code. Check the Grafana dashboard with a `project_id=<your-slug>` filter — events should appear within ~10 seconds.

### Step 6 — Skills are bundled

Skills are already in this template's `skills/` and `skills_private/`. Phase 5 SDK will replace this with canonical pulls from the-loom's catalog.

### Step 7 — Add Next.js scaffolding

This template ships skill+discipline scaffolding but not yet a `package.json`. To set up the web stack:

```powershell
# Initialize Next.js 16
npx create-next-app@latest . --typescript --tailwind --app --no-src-dir --import-alias "@/*"

# Add the rest of the recommended stack
npm install next-auth @auth/drizzle-adapter drizzle-orm pg @sentry/nextjs
npm install -D drizzle-kit @types/pg
```

### Step 8 — Run

```powershell
npm install
npm run dev
```

App at `http://localhost:3000`.

## Related

- Onboarding doc: [`the-loom/docs/howto/onboard-a-project.md`](https://github.com/Lizo-RoadTown/the-loom/blob/main/docs/howto/onboard-a-project.md)
- Scaffolder script: [`the-loom/scripts/new-loom-project.ps1`](https://github.com/Lizo-RoadTown/the-loom/blob/main/scripts/new-loom-project.ps1)
- Reference stack: [`humancensys-app`](https://github.com/Lizo-RoadTown/humancensys-app) (same stack pattern)
- Other templates: [`ux-starter`](https://github.com/Lizo-RoadTown/ux-starter), [`docs-agent`](https://github.com/Lizo-RoadTown/docs-agent)

## License

Apache 2.0 — see [LICENSE](LICENSE).
