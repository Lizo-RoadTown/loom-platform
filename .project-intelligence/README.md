# `.project-intelligence/` — Layer 3 instance state for loom-platform

This folder is the **project-local instance** layer of the three-layer engine model (per [Make_Skills](https://github.com/Lizo-RoadTown/Make_Skills) `docs/proposals/2026-05-31-three-layer-engine-spec.md`). It lives in this repo, not in any platform-side repo. It's what makes this project legible to the loom platform — registry, observatory, candidate generation — without coupling the platform code to this project's specifics.

## Contents

| Path | Purpose |
|---|---|
| `instances.json` | Canonical manifest of every project-local instance running on the shared core engine. loom-platform has one instance today (`loom-platform-dev`); future surfaces (e.g., a hosted production agent that visitors can chat with) would each get their own instance ID + folder. |
| `agent-profile.json` (top-level) | Agent kinds configured for this project. Today: one Claude Code session (the dev-time agent). |
| `project-context.json` (top-level) | The Project Registry's view of this project — slug, hostname, registration metadata. |
| `observatory-config.json` (top-level) | Telemetry destination (Grafana Cloud OTLP via .env). |
| `loom-platform-dev/` | The dev-time instance — what runs when the operator opens this repo in Claude Code to build the site. |
| `lessons-learned/` | Friction memories written during sessions, scoped to the project. Phase 2's local observer (planned in the-loom) reads from here. |
| `local-skills/` | Skill candidates this project is drafting that haven't been promoted to the platform catalog yet. |
| `promotion-candidates/` | Per the ratified 7-phase upskilling sequence: skills observed 3+ times stable, awaiting promotion to platform. |
| `workflow-candidates/` | Observed multi-step workflow patterns awaiting promotion. |

## Boundary

This folder is the **only** place this project owns its own instance state. The shared core engine (Make_Skills) and the platform substrate (the-loom) read from / write to this folder; they don't mirror it elsewhere. If a future surface gets its own instance (e.g., `loom-platform-prod` for the deployed site's embedded agent), it gets its own subdirectory and its own memory boundary — instances **must not** share memory or context with each other.

## Status

- **2026-06-12:** Initial seed committed. Phase 2 local observer (the daemon that writes candidates into the candidate subdirs based on session telemetry) has not yet shipped in the-loom — these dirs are currently empty awaiting Phase 2.
- Instance `loom-platform-dev` registered with the Project Registry: **pending**.
