# angelgce Worker Templates

## Overview
This repo contains opinionated full-stack templates for Cloudflare Workers. The main template lives in `template/`.

## Repo Structure
- `template/` — The starter template (backend, frontend, CI/CD, Claude config)
- `company/` — Internal projects (gitignored, not part of the template)

## Template Stack
- **Backend**: Hono + Cloudflare Workers + Drizzle ORM + Neon PostgreSQL + Workers AI
- **Frontend**: React 18 + Vite + Redux Toolkit + Tailwind CSS + TypeScript
- **CI/CD**: GitHub Actions → Cloudflare Pages & Workers

## Architecture

### Backend: Routes → Services → Repositories
- **Routes**: HTTP handlers + Zod validation. NO SQL, NO business logic.
- **Services**: Business logic. NO SQL queries.
- **Repositories**: SQL queries ONLY (Drizzle ORM).
- Entry point: `template/backend/src/worker.ts`
- DB schemas: `template/backend/src/database/drizzle-schema/*.schema.ts`
- AI routes: `template/backend/src/ai/` (Workers AI with Llama 3.1)

### Frontend: Modular Feature Structure
- `src/core/` — Shared state, hooks, API services
- `src/modules/` — Feature-based modules
- `src/shared/` — Reusable UI components
- Component sections (in order): local state, Redux selectors, custom hooks, computed values, effects, event handlers, render helpers, main render

### Responsive Design
- 3 breakpoints ONLY: mobile (base), `tablet:` (768px), `desktop:` (1280px)
- NEVER use sm, md, lg, xl, 2xl

### Import Order
1. React & Hooks
2. External Libraries
3. Core & Store
4. Utils & Services
5. Components
6. Constants & Types
7. Custom Hooks

## Rules
- NEVER create files/services without explicit request
- Use specialized agents for domain-specific tasks
- NO gradients — use solid colors or rgba
- Extract complex logic to custom hooks
- Keep components focused and single-responsibility
- All template changes go in `template/`, never in root
