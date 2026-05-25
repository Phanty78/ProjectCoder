# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

ProjectCoder is a web-based Tycoon game inspired by Game Dev Tycoon (see `doc/design.md`). The player starts as a coding novice and grinds skills (against time/money) to unlock missions. Missions require a set of skills plus a confidence level: success raises confidence and pays full, failure can lower both. The game loop is driven entirely by the data files below — gameplay balance lives in JSON, not code.

## Monorepo layout

Bun workspaces monorepo. Root `package.json` declares `"workspaces": ["backend", "frontend"]`.

- `backend/` — Bun server (`Bun.serve`, entry `src/index.ts`). Package name `@project-coder/backend`.
- `frontend/` — React UI via Bun HTML imports (scaffold only so far: `package.json` + `tsconfig.json`, no source yet).
- `data/` — game content, **not** a workspace. Source of truth for the game loop.
- `doc/` — `design.md` (game design) and `niveau.md` (internal tutoring-mode log; not project docs).

Dependencies, `bun.lock`, and `node_modules/` are **hoisted to the root** — always run `bun install` from the repo root, never inside a package (a stray `bun init`/`install` in a subfolder is what to avoid).

## Bun-only toolchain

This project uses Bun for everything (runtime, bundler, package manager, test runner). **Read `backend/CLAUDE.md` for the full mandate** — it forbids Node/npm/express/vite/jest/ws/pg/etc. in favor of Bun built-ins (`Bun.serve`, `bun:sqlite`, `Bun.sql`, `Bun.redis`, HTML imports, `bun test`). That mandate applies repo-wide, not just to the backend.

## Commands

Run from the repo root:

- `bun install` — install all workspaces (hoisted).
- `bun --hot backend/src/index.ts` — run the server with hot reload.
- `bun test` — run tests (`bun test <file>` for a single file, `bun test -t "<name>"` for a single test).
- `bun check` — Biome lint + format **with `--write`** (auto-fixes in place; it does not act as a read-only gate). `bun format` / `bun lint` likewise auto-write.

Note: `check`/`format`/`lint` are intentionally `--write` (solo project, auto-fix preferred). There is deliberately no read-only verify command — add one (drop `--write`) before wiring any CI or pre-commit gate, since `--write` always exits success.

## TypeScript config

Shared base in root `tsconfig.json`; each package `extends` it and overrides only what differs:
- `backend/tsconfig.json` → `lib: ["ESNext"]`, no DOM/JSX (server).
- `frontend/tsconfig.json` → adds `DOM`, `DOM.Iterable`, `jsx: "react-jsx"` (browser/React).

Tooling defaults (Biome): **tabs**, double quotes, organize-imports on. Don't reformat against these.

## Game data model (`data/`)

- `jobs.json` — array of tiers, each `{ preRequisites: number[], jobs: [...] }`. `preRequisites` lists job ids that must be done to unlock the tier; each job has `id`, `title`, `description`, `pay`, `confidenceGain`.
- `researchs.json` — skill tree keyed by skill name (`HTML`, `CSS`, `JavaScript`, …), each `{ id, title, description, skill-gain, time, pre-requisites: number[] }`. `pre-requisites` references skill ids, forming the unlock graph the player progresses through.

When changing game balance or progression, edit these files — the prerequisite ids wire the dependency graph between skills and job tiers.
