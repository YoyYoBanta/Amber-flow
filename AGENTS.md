# AGENTS.md — POAI (Partnership Operations AI Assistant)

Start here. This file gives a coding agent everything it needs to pick up work without reading old chats.

## What this is
POAI turns partner requests posted in Slack into WhatsApp replies that an account manager approves before they go out. Slack message → Groq extracts the details (incl. `context` and `action_type`) → draft lands in the Approval Queue → approved messages are sent through the UltraMsg WhatsApp API.

Stack: Next.js 15 (App Router), TypeScript, Tailwind, shadcn/ui, Supabase (Postgres + Auth), Groq, Vercel.

## Read in this order
1. `context.md` — requirements, scope and design constraints (original brief: `problem.txt`)
2. `architecture.md` — how the pieces fit together
3. `implementation-plan.md` — phases 1–11 and what each one covers
4. `decision.md` — decisions already taken; don't reopen them without a reason
5. `WA-architecture.md`, `WA-implementation.md` — the WhatsApp sending path
6. `security.md`, `evals.md` — as needed

## Current status
- Live on Vercel; handed to Amber's DevOps team, who run it on staging.
- Done: Approval Queue split into "New Tags" and "Follow-up Messages" tabs; full-width dashboard layout; interactive Settings page (Account, Notifications, Security) with explicit Edit → Save; header shows the signed-in user's name from Supabase.
- Done: double-send protection on queue actions; latency columns `slack_received_at`, `draft_ready_at`, `sent_at`, used by the "Average Response Time" KPI; pending-actions feed capped at 8; dashboard uses `force-dynamic`.
- Dropped: the custom Baileys gateway. Sending goes through UltraMsg directly.

## Working rules
- Make changes additively. Never break the working Slack webhook → queue → WhatsApp dispatch path.
- Run `npm run build` before pushing; Vercel builds are strict about ESLint and TypeScript errors.
- When you finish a piece of work, update the "Current status" section above instead of creating new log files.
