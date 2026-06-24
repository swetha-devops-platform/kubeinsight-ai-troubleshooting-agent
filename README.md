# KubeInsight AI - AI-Powered Kubernetes Troubleshooting Agent

**Investigate Faster. Diagnose Smarter. Resolve Confidently.**

KubeInsight AI is an intelligent Kubernetes troubleshooting platform that automates cluster investigation, root cause analysis, severity classification, remediation guidance, incident intelligence, and report generation.

Built for DevOps Engineers, SREs, Platform Engineers, and Cloud Teams who want to reduce Kubernetes troubleshooting time and improve operational efficiency.

---

# Why KubeInsight AI?

Troubleshooting Kubernetes issues often requires engineers to:

* Run multiple kubectl commands
* Check logs manually
* Analyze events
* Inspect deployments
* Investigate networking
* Correlate failures
* Determine root cause

KubeInsight AI automates this workflow by acting as an AI-powered Kubernetes Investigation Assistant.

Instead of spending 30–60 minutes debugging, engineers receive a structured diagnosis within seconds.

---

# Project Structure

```text
ai-kubernetes-agent/
│
├── frontend/                        # Next.js · TypeScript · Tailwind CSS
│   ├── app/                         # Pages and routes
│   ├── components/                  # UI components (Incident Intelligence Dashboard)
│   ├── hooks/                       # Custom React hooks
│   ├── services/                    # API service calls to backend
│   ├── types/                       # TypeScript type definitions
│   ├── Dockerfile
│   ├── next.config.js
│   └── tailwind.config.ts
│
├── investigation-service/           # FastAPI · Python · Uvicorn
│   ├── app/
│   │   ├── api/                     # /investigate · /clusters · /health endpoints
│   │   ├── ai/                      # Prompt builder · LLM reasoning · Confidence engine
│   │   ├── investigation/           # Pod · Logs · Events · Deployment · Network inspectors
│   │   ├── reports/                 # Investigation report generator ★
│   │   └── storage/                 # InsForge history · Similar incident finder ★
│   ├── requirements.txt
│   └── Dockerfile
│
├── prompts/                         # Development prompt files
├── docs/                            # Documentation
├── docker-compose.yml
├── imagepullbackoff.yaml            # Demo workload
├── .env
└── README.md
```

---

# Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                    Kubernetes Cluster                        │
│         Pods · Deployments · Services · Events · Logs       │
│           Where failures happen — evidence lives here        │
└──────────────────────────┬──────────────────────────────────┘
                           │  kubectl / Kubernetes API
                           ▼
┌─────────────────────────────────────────────────────────────┐
│          Investigation Layer  (app/investigation)            │
│                                                              │
│  Pod Inspector · Logs Collector · Events Analyzer            │
│  Deployment Inspector · Network Inspector                    │
│                                                              │
│  Collects all troubleshooting signals from the cluster       │
└──────────────────────────┬──────────────────────────────────┘
                           │  Structured evidence package
                           ▼
┌─────────────────────────────────────────────────────────────┐
│              AI Kubernetes Agent  (app/ai)                   │
│                                                              │
│  Prompt Builder → LLM Reasoning (OpenRouter API)             │
│  Claude · GPT · DeepSeek                                     │
│                                                              │
│  Root Cause Analyzer · Fix Recommendation Engine             │
│  ★ Severity Classifier  (CRITICAL / HIGH / MEDIUM / LOW)    │
│  ★ Confidence Scoring   (Evidence-based rule override)       │
│  ★ Report Generator     (Downloadable investigation PDF)     │
└──────────────────────────┬──────────────────────────────────┘
                           │  Investigation result
                           ▼
┌─────────────────────────────────────────────────────────────┐
│           InsForge Backend  (BaaS — app/storage)             │
│                                                              │
│  Authentication  (Login · Signup · Protected routes)         │
│  API Layer       (Trigger investigations · Return analysis)  │
│  ★ History Store (Cluster · Namespace · Severity · Score)   │
│  ★ Realtime      (PostgreSQL RPC wrapper — custom built)     │
└──────────────────────────┬──────────────────────────────────┘
                           │  API response
                           ▼
┌─────────────────────────────────────────────────────────────┐
│          Frontend Dashboard  (Next.js — frontend/app)        │
│                                                              │
│  Cluster Selector (3 Minikube contexts)                      │
│  Live Progress Checklist · Diagnosis Panel · PDF Download    │
│  ★ Incident Intelligence Dashboard                           │
│    Recent investigations · Similar incident finder           │
│    Clear history · Severity badges                           │
└─────────────────────────────────────────────────────────────┘

★ = Enhancements built independently (not in the original concept)
```

---

# Investigation Workflow

```text
Step 1 — User opens http://localhost:3000
         InsForge auth → Login / Signup / Email verify
                │
                ▼
Step 2 — Select cluster + namespace
         Dropdown fetches 3 Minikube contexts from GET /clusters
                │
                ▼
Step 3 — Click "Run Investigation"
         POST /investigate → FastAPI backend triggered
                │
                ▼
Step 4 — Evidence Collection Engine
         ┌──────────────────────────────────────┐
         │ Pods · Logs · Events · Services ·    │
         │ Deployments · Endpoints · Networking │
         │ ★ Live progress streamed to UI        │
         └──────────────────────────────────────┘
                │
                ▼
Step 5 — AI Agent Analysis
         Evidence → Prompt Builder → OpenRouter LLM
         ┌──────────────────────────────────────┐
         │ Root cause detection                 │
         │ ★ Severity classification            │
         │ ★ Evidence-based confidence scoring  │
         │ ★ Actionable kubectl commands        │
         └──────────────────────────────────────┘
                │
                ▼
Step 6 — Diagnosis displayed on dashboard
         Root cause · Suggested fix · kubectl commands
         ★ Severity card · ★ Confidence score
                │
                ▼
Step 7 — ★ Incident Intelligence (stored in InsForge)
         ┌──────────────────────────────────────┐
         │ ★ Recent investigations table        │
         │ ★ Similar incident finder            │
         │ ★ Clear history button               │
         └──────────────────────────────────────┘
                │
                ▼
Step 8 — ★ Download Investigation Report
         Root cause · Evidence · Fix · kubectl commands · Prevention tips
```

---

# Key Features

##  Kubernetes Investigation Engine

Automatically collects:

* Pods
* Logs
* Events
* Deployments
* Services
* Endpoints
* Namespace Information
* Restart Counts
* Cluster Information

Acts like a Junior DevOps Engineer gathering evidence before diagnosis.

---

##  AI-Powered Root Cause Analysis

Detects common Kubernetes failures:

* ImagePullBackOff
* ErrImagePull
* CrashLoopBackOff
* OOMKilled
* FailedScheduling
* Pending Pods
* Service Selector Mismatch
* Missing Endpoints
* Deployment Failures
* Kubernetes API Connectivity Issues

---

##  Severity Classification Engine 

Every incident is automatically classified as:

```text
CRITICAL
HIGH
MEDIUM
LOW
```

Based on workload impact, service availability, deployment health, pod failures, and cluster-wide effects.

---

## Actionable Remediation Commands 

Instead of generic recommendations, KubeInsight AI generates executable commands:

```bash
kubectl describe pod nginx-abc
kubectl logs nginx-abc
kubectl rollout restart deployment nginx
kubectl get events --sort-by=.metadata.creationTimestamp
```

---

## Similar Incident Finder 

Searches historical investigations and displays:

* Similar incidents
* Previous root causes
* Historical fixes
* Resolution patterns

This significantly reduces repetitive troubleshooting efforts.

---

## Investigation History 

Stores every run with:

* Cluster
* Namespace
* Root Cause
* Severity
* Confidence Score
* Timestamp

---

## Investigation Report Generator 

Generates downloadable investigation reports containing:

* Investigation Summary
* Root Cause
* Severity
* Confidence Score
* Evidence Collected
* Suggested Fix
* kubectl Commands
* Prevention Recommendations

Useful for audits, postmortems, and knowledge sharing.

---

## Incident Intelligence Dashboard 

* Recent Investigations — track previously executed investigations
* Similar Incidents — identify recurring failures quickly
* Clear History — remove stored investigation data instantly

---

# Supported Failure Scenarios

The Kubernetes Investigation Engine is capable of identifying a wide range of Kubernetes operational failures, including:

- ImagePullBackOff
- ErrImagePull
- CrashLoopBackOff
- OOMKilled
- Pending Pods
- FailedScheduling
- FailedPull
- BackOff Events
- Deployment Failures
- Service Selector Mismatch
- Missing Endpoints
- Kubernetes API Connectivity Issues

For each detected issue, KubeInsight AI performs evidence collection, AI-powered diagnosis, severity classification, confidence scoring, remediation command generation, and investigation report creation.

---

# Tech Stack

## Frontend
* Next.js · TypeScript · Tailwind CSS

## Backend
* FastAPI · Python · Uvicorn · Pydantic · HTTPX · Loguru

## AI Layer
* OpenRouter · InsForge · LLM Reasoning (Claude / GPT / DeepSeek)

## Kubernetes
* kubectl · Minikube (3-cluster multi-context setup)

## Infrastructure
* Docker · Docker Compose


---

# Demo Scenarios

## ImagePullBackOff

```bash
kubectl create deployment imagepullbackoff-demo --image=nginx:invalidtag123
```

Expected Diagnosis: `Container image pull failure`

---

## CrashLoopBackOff

```bash
kubectl create deployment crashloop-demo --image=busybox -- /bin/sh -c "exit 1"
```

Expected Diagnosis: `Application container repeatedly crashing`

---

## Pending Pod

```bash
kubectl run pending-demo --image=nginx --requests='cpu=9999'
```

Expected Diagnosis: `Pod scheduling failure`

---

# What Makes KubeInsight AI Different?

- The original AI Kubernetes Agent concept covered the core investigation loop — collecting Kubernetes evidence and passing it to an LLM for root cause analysis.

- KubeInsight AI takes that foundation and adds a full operational layer on top.

## Shared Foundation (also in original)

| Capability                     | Status  |
| ------------------------------ | ------- |
| Root Cause Analysis            | Both |
| Suggested Fixes                | Both |
| Kubernetes Investigation Layer | Both |

## KubeInsight AI Enhancements

| Enhancement                         | What It Does |
| ----------------------------------- | ------------ |
| **★ Severity Classification**       | Auto-labels every incident as CRITICAL / HIGH / MEDIUM / LOW |
| **★ Actionable Remediation Commands** | Generates real executable `kubectl` commands, not just text |
| **★ Similar Incident Finder**       | Searches historical investigations to surface matching failures |
| **★ Investigation History**         | Stores every run with cluster, namespace, severity, confidence, timestamp |
| **★ Report Generator**              | Downloadable investigation report for audits and postmortems |
| **★ Incident Intelligence Dashboard** | Recent runs, similar incidents, severity badges in one UI |
| **★ Evidence-Based Confidence Scoring** | Rule-based override so strong evidence never shows 0% confidence |
| **★ Minikube Multi-Cluster Setup**  | 3-context local cluster setup vs single-cluster Kind in original |

---

# InsForge — Backend as a Service

KubeInsight AI uses **InsForge** as its Backend-as-a-Service (BaaS) platform, similar in concept to Supabase or Firebase, for auth, storage, and realtime features.

| Feature                        | InsForge Role |
| ------------------------------ | ------------- |
| **Authentication**             | Login, signup, email verify, JWT-protected routes |
| **Investigation History**    | Stores every investigation run as a queryable record |
| **Similar Incident Finder**  | Queries stored records to surface matching historical incidents |
| **Realtime Progress Updates** | Streams live investigation steps to the frontend |
| **Clear History**            | Deletes stored investigation records on demand |

- **Key engineering decision:** InsForge Realtime has no plain REST publish endpoint.

- A custom **PostgreSQL RPC wrapper function** was built to work around this constraint,

- allowing the backend to push live progress events to the frontend during active investigations.

---

# Acknowledgements

This project was inspired by the **AI Kubernetes Agent** tutorial by **Abhishek Veeramalla**, a well-known DevOps educator whose work has helped thousands of engineers learn Kubernetes and cloud-native tooling.

## How KubeInsight AI Differs from the Original

Abhishek's version was built using **Cursor** as the AI coding assistant and **Kind** for local Kubernetes setup. KubeInsight AI was built independently using **GitHub Copilot / Codex** and **Minikube** with a multi-cluster, 3-context local setup.

Beyond the toolchain, KubeInsight AI extends the original concept with a full operational layer designed and implemented independently:

**Severity Classification** — Every incident is automatically labeled as CRITICAL, HIGH, MEDIUM, or LOW based on workload impact and cluster health.

**Actionable Remediation Commands** — Instead of generic text suggestions, the agent generates real executable `kubectl` commands the engineer can run immediately.

**Similar Incident Finder** — The platform searches historical investigations to surface matching failure patterns, reducing repetitive troubleshooting.

**Investigation History** — Every investigation run is stored with cluster, namespace, root cause, severity, confidence score, and timestamp for future reference.

**Report Generator** — Produces a downloadable investigation report suitable for audits, postmortems, and team knowledge sharing.

**Incident Intelligence Dashboard** — A full UI layer showing recent investigations, similar incidents, and severity badges in one place.

**Evidence-Based Confidence Scoring** — A rule-based override layer ensures strong Kubernetes evidence never produces a misleading 0% confidence score from the LLM.

**Authentication** — Full InsForge-powered auth with login, signup, email verification, and protected dashboard routes.

**BaaS Integration** — InsForge used as the backend service layer, including a custom PostgreSQL RPC wrapper built to work around InsForge Realtime's lack of a plain REST publish endpoint.

- All code in this repository is independently written.
  
- KubeInsight AI is **not a fork** — it is built from scratch using the concept as a learning foundation and extended significantly beyond the original scope.

---

# Author

**Swetha Ravi**

System Engineer | DevOps Engineer

- Passionate about Kubernetes  and DevOps


---

## If you found this project useful, consider giving it a star!

**KubeInsight AI — Turning Kubernetes Troubleshooting into an Intelligent Investigation Experience.**
