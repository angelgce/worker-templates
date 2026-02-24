# angelgce Worker Templates

Opinionated full-stack templates for building production-ready applications on Cloudflare Workers.

## What's Inside

### `template/`

A ready-to-use starter with everything wired up:

- **Backend** — Hono + Cloudflare Workers + Drizzle ORM + Neon PostgreSQL + Workers AI (Llama 3.1)
- **Frontend** — React 18 + Vite + Redux Toolkit + Tailwind CSS + TypeScript
- **CI/CD** — GitHub Actions for auto-deploy to Cloudflare Pages & Workers
- **Claude Code** — 5 agents + 40+ skills for AI-assisted development

## Quick Start

```bash
# Clone the template into your new project
git clone https://github.com/angelgce/worker-templates.git my-project
cd my-project/template

# Install dependencies
npm run install:all

# Configure environment
cp backend/.env_example backend/.dev.vars

# Start developing
npm run dev:back   # Backend on :8080
npm run dev:front  # Frontend on :5173
```

See [`template/README.md`](template/README.md) for full setup, architecture, and deployment docs.

## License

MIT
