# Worker Templates

Full-stack template for building applications on Cloudflare Workers with AI capabilities.

## Stack

| Layer | Tech |
|-------|------|
| **Backend** | [Hono](https://hono.dev) + Cloudflare Workers + Drizzle ORM + Neon PostgreSQL |
| **Frontend** | React 18 + Vite + Redux Toolkit + Tailwind CSS + TypeScript |
| **AI** | Cloudflare Workers AI (Llama 3.1 8B Instruct) |
| **CI/CD** | GitHub Actions → Cloudflare Pages & Workers |

## Project Structure

```
├── backend/
│   ├── src/
│   │   ├── ai/              # Workers AI service & routes
│   │   ├── auth/             # JWT auth middleware
│   │   ├── database/         # Drizzle schemas
│   │   ├── lib/              # DB connection, JWT helpers
│   │   └── worker.ts         # Entry point (Hono app)
│   ├── wrangler.toml         # Cloudflare Workers config
│   └── drizzle.config.ts
├── frontend/
│   ├── src/
│   │   ├── core/             # Shared hooks, store, services
│   │   ├── modules/          # Feature modules
│   │   └── shared/           # Reusable components
│   └── vite.config.ts
├── .claude/
│   ├── agents/               # Claude Code agents
│   └── skills/               # Claude Code skills (40+)
└── .github/workflows/        # CI/CD deploy pipelines
```

## Getting Started

### Prerequisites

- Node.js 20+
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/) (`npm i -g wrangler`)
- Cloudflare account

### Setup

```bash
# Install all dependencies
npm run install:all

# Configure backend environment
cp backend/.env_example backend/.dev.vars
```

Edit `backend/.dev.vars` with your values:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/app_dev
JWT_SECRET=your-jwt-secret
FRONTEND_HOST=http://localhost:5173
```

### Development

```bash
# Start backend (port 8080)
npm run dev:back

# Start frontend (port 5173)
npm run dev:front
```

### Database

```bash
cd backend

# Generate migrations from schema changes
npm run drizzle:generate

# Apply migrations
npm run drizzle:migrate

# Open Drizzle Studio
npm run drizzle:studio
```

## AI Endpoints

Workers AI is pre-configured with the `@cf/meta/llama-3.1-8b-instruct` model.

### POST /api/ai/completion

```json
{
  "prompt": "Explain serverless computing in one sentence"
}
```

### POST /api/ai/chat

```json
{
  "messages": [
    { "role": "system", "content": "You are a helpful assistant." },
    { "role": "user", "content": "What is Cloudflare Workers?" }
  ]
}
```

## Architecture

### Backend Pattern: Routes → Services → Repositories

- **Routes** — HTTP handlers + Zod validation. No SQL, no business logic.
- **Services** — Business logic. No SQL queries.
- **Repositories** — SQL queries only (Drizzle ORM).

### Frontend Pattern: Modular Feature Structure

- `src/core/` — Shared state, hooks, API services
- `src/modules/` — Feature-based modules (each self-contained)
- `src/shared/` — Reusable UI components

## Deployment

### GitHub Actions (automatic)

Pushes to `main` or `dev` auto-deploy via GitHub Actions:

- **Frontend** → Cloudflare Pages
- **Backend** → Cloudflare Workers (with DB migrations)

### Required GitHub Secrets

| Secret | Description |
|--------|-------------|
| `CLOUDFLARE_API_TOKEN` | Cloudflare API token |
| `CLOUDFLARE_ACCOUNT_ID` | Cloudflare account ID |
| `DATABASE_URL_PROD` | Production database URL |
| `DATABASE_URL_DEV` | Development database URL |
| `JWT_SECRET` | JWT signing secret |

### Required GitHub Variables

| Variable | Description |
|----------|-------------|
| `PROJECT_NAME` | Base project name for Cloudflare |
| `BACKEND_URL_PROD` | Production backend URL |
| `BACKEND_URL_DEV` | Dev backend URL |
| `FRONTEND_URL_PROD` | Production frontend URL |
| `FRONTEND_URL_DEV` | Dev frontend URL |

### Manual Deploy

```bash
# Backend
cd backend
npm run deploy:dev      # Deploy to dev
npm run deploy:prod     # Deploy to production

# Frontend (via wrangler pages)
cd frontend
npm run build
npx wrangler pages deploy dist --project-name=your-project
```

## Claude Code Integration

This template includes 5 specialized agents and 40+ skills for [Claude Code](https://claude.com/claude-code):

**Agents:** backend-engineer, frontend-designer, cloudflare-expert, bug-fixer, responsive-layout

**Key Skills:** cloudflare, wrangler, hono-routing, durable-objects, tailwind-design-system, systematic-debugging, test-driven-development, performance, and more.

## License

MIT
