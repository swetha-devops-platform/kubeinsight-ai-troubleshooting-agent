# 01-prompt-project-setup.md

## Context

We are building an **AI Kubernetes Troubleshooting Agent**.

Architecture:

```text
Kubernetes Cluster (pods, deployments, services, events, logs)
        ↓ kubectl / Kubernetes API
Investigation Layer (Pod Inspector, Logs Collector, Events Analyzer,
                      Deployment Inspector, Network Inspector,
                      Namespace Scope Analyzer)
        ↓ Structured Investigation Data
AI Kubernetes Agent (Prompt Builder, LLM Reasoning via OpenRouter,
                      Root Cause Analyzer, Fix Recommendation Engine,
                      Confidence Scoring, Incident Severity Engine)
        ↓ Investigation Result
InsForge Backend (Auth, API Layer, Investigation History,
                   Realtime Updates, Investigation Knowledge Base)
        ↓ API Response
Frontend Dashboard (Incident, Severity, Confidence, Root Cause, Fix)
        ↓
InsForge Deployment (Frontend + Backend + Public URL)
```

This is an **on-demand troubleshooting system**.

Example flow:

```text
User clicks "Investigate Cluster"
        ↓
Frontend calls Investigation Service
        ↓
Investigation Layer inspects the cluster
        ↓
AI Kubernetes Agent reasons over the evidence
        ↓
Result is written to InsForge (history + realtime)
        ↓
Diagnosis shown on the Frontend Dashboard
```

We are **NOT building a Kubernetes controller/operator**.

A note on roles, since this differs from a typical FastAPI+Postgres setup:
- The **Investigation Layer** and **AI Kubernetes Agent** need a runtime that can run `kubectl`/talk to the Kubernetes API and call an LLM — that's a Python service we own (called `investigation-service` below).
- **InsForge** is the agent-native backend platform that *is* our "InsForge Backend" layer — it provides Auth, the Database (Investigation History + Investigation Knowledge Base), Realtime Updates, and Site Deployment. We do not hand-roll these ourselves.

---

## Goal

Set up the project foundation.

Create:
- `investigation-service` (Python/FastAPI) — will later host the Investigation Layer + AI Agent
- Next.js frontend — will later host the Frontend Dashboard
- Docker setup for local dev
- Environment variables, including placeholders for InsForge
- Basic folder structure
- Health endpoint

Do NOT implement Kubernetes logic, AI reasoning, or InsForge auth/database/realtime logic yet.

---

## Tech Stack

**Investigation Service:**
- FastAPI
- Python 3.12+
- Uvicorn
- Pydantic
- Loguru
- HTTPX
- `kubernetes` Python client (installed, unused — placeholder only)

**Frontend:**
- Next.js
- TypeScript
- Tailwind CSS
- Axios / React Query
- InsForge JS SDK (installed, unused — placeholder only)

**Backend Platform:**
- InsForge (cloud project at insforge.dev) — provides Auth, Database, Realtime, Model Gateway, Site Deployment

**Infrastructure:**
- Docker
- Docker Compose (for `investigation-service` + `frontend` only — InsForge is managed separately as a linked cloud project, not in this compose file)

---

## Project Structure

Create a monorepo:

```text
ai-kubernetes-agent/
├── investigation-service/
├── frontend/
├── docs/
├── prompts/
├── docker-compose.yml
└── README.md
```

`investigation-service` should include placeholders for:

```text
api/
core/
kubernetes/
ai/
services/
models/
```

`frontend` should include placeholders for:

```text
components/
services/
hooks/
types/
```

Use placeholder implementations only.

Example:

```python
def inspect_pods():
    pass
```

---

## Investigation Service Requirements

Create FastAPI app.

Add:

```text
GET /health
```

Response:

```json
{
  "status": "healthy",
  "service": "investigation-service"
}
```

Enable:
- CORS
- logging
- env loading

Keep code simple and modular.

---

## Frontend Requirements

Create a minimal homepage — but make it look like a real product, not a wireframe.

Design direction:
- Dark theme: deep slate/near-black background (e.g. `slate-950` → `slate-900` gradient)
- Centered hero layout, generous vertical spacing
- Small pill badge above the headline: `⎈ AI-Powered Diagnostics` in a muted blue, soft glow border
- Headline: **AI Kubernetes Agent** — large, bold, white, slightly tight letter-spacing
- Subheadline: *"Troubleshoot Kubernetes failures with AI-driven root cause analysis"* — slate-400, smaller, max-width so it wraps nicely
- Primary CTA button: **Investigate Cluster**
  - Gradient fill (blue → violet)
  - Rounded-full, subtle shadow/glow on hover, slight scale-up on hover
  - Icon (e.g. a small radar/scan icon) to the left of the label
- Status indicator below the CTA: a small pill showing **System Status: Ready** with a pulsing green dot
- Faint decorative background detail — a subtle grid pattern or soft radial glow behind the hero — low opacity so it doesn't compete with the text
- Footer line, small and muted: *"Powered by InsForge"*

This is still a **static placeholder page** — no real API calls, no real status check. Just the visual shell, built so later prompts can wire it up without restyling it.

---

## InsForge Requirements

- Create/link an InsForge project for this app (via `npx @insforge/cli login` or the InsForge MCP connection in the editor).
- Confirm the project is reachable — note the Project URL and API key, store them as env placeholders (see below).
- Do NOT configure:
  - Auth providers
  - Database schema (Investigation History, Investigation Knowledge Base tables)
  - Realtime channels
  - Model Gateway routes

These get wired up in later prompts. This step only establishes the link between the project and InsForge.

---

## Environment Variables

`investigation-service/.env`:

```env
OPENROUTER_API_KEY=
OPENROUTER_MODEL=
KUBECONFIG_PATH=
INSFORGE_PROJECT_URL=
INSFORGE_API_KEY=
```

`frontend/.env.local`:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
NEXT_PUBLIC_INSFORGE_PROJECT_URL=
NEXT_PUBLIC_INSFORGE_ANON_KEY=
```

---

## Docker Requirements

Create Dockerfiles for:
- `investigation-service`
- `frontend`

Create docker-compose:

```text
investigation-service → port 8000
frontend               → port 3000
```

InsForge is not part of this compose file — it's a linked cloud project (or self-hosted separately via its own `docker-compose.prod.yml` if you choose self-hosting later).

---

## Constraints

DO NOT implement:
- kubectl / Kubernetes API logic
- AI reasoning / prompt building
- OpenRouter calls
- InsForge auth, database schema, realtime channels, or model gateway wiring
- Confidence scoring or severity engine logic

Only set up the foundation.

DO NOT BREAK EXISTING CODE in future implementations.

Keep everything beginner-friendly and production-style.

---

## Expected Result

I should be able to run:

```bash
docker compose up --build
```

Access:

```text
http://localhost:3000
http://localhost:8000/health
```

The frontend should look polished and intentional — dark, modern, gradient CTA — even though nothing is functional yet.
