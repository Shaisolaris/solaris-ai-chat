# Solaris AI Chat

A production-ready AI assistant built on Next.js 15, the Vercel AI SDK, shadcn/ui, and Postgres. Solaris AI Chat ships with authentication, persistent conversations, streaming responses, artifact editing (code / text / sheets / images), and multi-model provider support out of the box.

**Live demo:** https://solaris-ai-chat.vercel.app _(deployed by Shai — see setup below)_

## Features

- Streaming chat with multi-turn context and persistent history
- Multi-provider model support via Vercel AI Gateway (OpenAI, Anthropic, xAI, Google, etc.)
- Authenticated sessions (NextAuth v5)
- Artifact workspace: code, text, spreadsheets, and images
- File uploads via Vercel Blob
- Rate limiting and session storage via Redis
- Postgres-backed chat and user persistence (Drizzle ORM)
- shadcn/ui components on Tailwind CSS
- Dark mode, full mobile responsiveness

## Tech stack

- Next.js 15 (App Router, RSC, Server Actions)
- React 19 + TypeScript
- Vercel AI SDK 5
- shadcn/ui + Tailwind CSS 4
- NextAuth v5
- Drizzle ORM + Postgres
- Vercel Blob (file storage) + Redis (rate limiting)

## Setup (one-time)

This repo needs external services to run. The fastest path is to deploy it directly to Vercel — Vercel will provision Postgres, Blob, and Redis for you automatically.

### Option A — Deploy to Vercel (recommended)

1. Push this repo to GitHub (already done).
2. Go to https://vercel.com/new and import `Shaisolaris/solaris-ai-chat`.
3. During import, add these environment variables:

   | Variable                | Where to get it                                          |
   | ----------------------- | -------------------------------------------------------- |
   | `AUTH_SECRET`           | `openssl rand -base64 32`                                |
   | `AI_GATEWAY_API_KEY`    | https://vercel.com/ai-gateway (or set an OpenAI API key) |
   | `POSTGRES_URL`          | Auto-provisioned by Vercel Postgres add-on               |
   | `BLOB_READ_WRITE_TOKEN` | Auto-provisioned by Vercel Blob add-on                   |
   | `REDIS_URL`             | Auto-provisioned by Vercel Redis add-on                  |

4. Deploy. Vercel runs `tsx lib/db/migrate` as part of the build, which creates the tables automatically.

### Option B — Run locally

```bash
pnpm install
cp .env.example .env.local
# fill in the values in .env.local
pnpm db:migrate
pnpm dev
```

Open http://localhost:3000.

## Project structure

- `app/` — Next.js App Router pages and API routes
- `components/` — UI components (shadcn/ui primitives, chat surface, artifact editor)
- `lib/db/` — Drizzle schema, migrations, and queries
- `lib/ai/` — Model provider configuration and streaming helpers
- `artifacts/` — Artifact type definitions (code, text, sheet, image)

## Credits

Based on the open-source [`vercel/ai-chatbot`](https://github.com/vercel/ai-chatbot) template, rebranded for the Solaris product suite.
