# Waselify

A marketplace for business automations. A client signs in, browses ready-made
workflows (lead generation, WhatsApp chatbots, Gmail auto-responders, invoice
collection, RAG chatbots and more), requests access, and then runs and monitors
each one from its own dashboard. The workflows themselves run on
[n8n](https://n8n.io).

## How it fits together

```
React app (Vite)  --->  Express API  --->  n8n instance
       |                     |
       +------ Supabase -----+   (auth and Postgres)
```

- **Frontend** (`src/`): React 18, TypeScript, Tailwind, shadcn/ui, TanStack
  Query, Recharts. English and Arabic through `src/lib/i18n.ts`.
- **Backend** (`backend/`): Express API that sits between the app and n8n, so
  the n8n API key never reaches the browser. Routes for workflows, executions,
  templates, imports, webhooks, admin and health.
- **Auth**: Supabase. The API verifies the bearer token on each request in
  `backend/middleware/auth.js`. The Google OAuth token exchange runs server
  side in `api/oauth/google.js`, so the client secret stays off the browser.
- **API hardening**: `helmet`, CORS restricted to the frontend origin, and
  `express-rate-limit`, all set up in `backend/server.js`.

## What is in the app

- Workflow marketplace with a sample dashboard for every workflow, so a visitor
  can see what they would get before requesting access
- A dashboard per workflow for clients who have access
- Access requests and custom workflow requests
- An admin dashboard for approving requests and controlling workflows
- Sign up, sign in, password reset, Google OAuth

## Run it locally

Frontend:

```bash
npm install
cp env.example .env      # then fill in your Supabase and Google values
npm run dev              # http://localhost:8080
```

Backend:

```bash
cd backend
npm install
cp env.example .env      # Supabase, n8n URL and API key, JWT secret
npm run dev              # http://localhost:3001
```

You need a Supabase project and a running n8n instance. See
[`backend/README.md`](backend/README.md) for the API reference.

## Secrets

No credentials are stored in this repository. Both `.env` files are ignored by
git, and the `env.example` files contain placeholders only. In production the
values are set in the hosting provider's environment settings.
