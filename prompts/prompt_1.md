# 01-prompt-project-setup.md

## Context

We are building an **AI Kubernetes Troubleshooting Agent**.

# Architecture:

```
Frontend Dashboard
        ↓
FastAPI Backend (Orchestrator)
        ↓
Kubernetes Investigation Layer
        ↓
AI Kubernetes Agent
        ↓
LLM Reasoning (OpenRouter via InsForge)
        ↓
Root Cause + Suggested Fix
        ↓
Severity Classification
        ↓
Actionable kubectl Remediation Commands
        ↓
Similar Incident Finder ⭐
        ↓
Investigation History Storage
        ↓
Investigation Report Generator ⭐
        ↓
Frontend Diagnosis Dashboard
```

This is an **on-demand troubleshooting system**.

---

# Example flow:

```text
User Clicks "Run Investigation"
                ↓
FastAPI Backend Receives Request
                ↓
Kubernetes Investigation Layer
                ↓
AI Kubernetes Agent - Builds Investigation Prompt
                ↓
OpenRouter LLM - Analyzes Kubernetes Evidence
                ↓
Search Similar Previous Incidents
                ↓
Store Investigation History
                ↓
Generate Investigation Report
                ↓
Display Results on Dashboard
```

We are **NOT building a Kubernetes controller/operator**.

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

**Frontend:**
- Next.js
- TypeScript
- Tailwind CSS
- Axios 
- React Query

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

## Backend Requirements

Create FastAPI app.

Add:

```text
GET /health
```

Response:

```json
{
  "status": "healthy",
  "service": "ai-k8s-agent"
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

## Environment Variables

`backend`:

```env
OPENROUTER_API_KEY=
OPENROUTER_MODEL=
KUBECONFIG_PATH=
```

`frontend/.env.local`:

```env
NEXT_PUBLIC_API_BASE_URL=http://localhost:8000
```

---

## Docker Requirements

Create Dockerfiles for:
- `backend`
- `frontend`

Create docker-compose:

```text
backend → port 8000
frontend               → port 3000
```


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
