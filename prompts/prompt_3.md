# 03-prompt-ai-reasoning-engine.md

## Context

The Kubernetes Investigation Layer is complete (Prompt 2).

We can now collect Kubernetes troubleshooting evidence 

```text
namespaces   
pods         
logs         
events       
deployments  
network      
```

# Architecture:

```text

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

Goal:

We now want to make the system intelligent.

The AI agent should behave like a **Senior Kubernetes SRE**.

It should:

- Understand Kubernetes failures
- Correlate logs, events, deployment state, networking, and namespace scope
- Identify the root cause
- Suggest Kubernetes fixes
- Generate confidence score
- Classify incident severity
- Generate actionable kubectl remediation commands

Important:

Use OpenRouter API Key provided via InsForge.

Do not hardcode secrets.

---

## Goal

Build the **AI Kubernetes Agent** inside
`investigation-service/app/ai/`.

Implement:

```text
Prompt Builder
LLM Client
Root Cause Analyzer
Fix Recommendation Engine
Confidence Engine
Incident Severity Engine
```

The AI layer consumes the investigation payload generated in Prompt 2.

---

## Requirements

### 1. Prompt Builder

The system prompt should make the LLM behave like:

```text
Senior Kubernetes SRE
```

The prompt must include:

```text
Affected Namespaces
Affected Workloads
Pod Status
Logs
Events
Deployment Health
Networking Findings
Namespace Scope Analysis
```

The AI must return:

```text
1. Root Cause

2. Explanation

3. Suggested Fix

4. kubectl Remediation Commands

5. Prevention Recommendation

6. Confidence Score

7. Severity
   (Critical / High / Medium / Low)
```


Prompt should be structured and deterministic. Avoid vague answers.

---

### 2. LLM Client

Use:

```text
OpenRouter
```

Authentication:

```text
OpenRouter API Key from InsForge
```

Read from:

```env
OPENROUTER_API_KEY=
OPENROUTER_MODEL=
```

Requirements:

- Use HTTPX
- Add timeout handling
- Handle API failures gracefully
- Add retries 
- Log errors cleanly

Do not expose secrets.

Keep implementation simple and explainable.

---

### 3. Root Cause Analyzer

Consume investigation payload.

Example input:

```
{
  "pods": {
    "status": "CrashLoopBackOff"
  },
  "logs": {
    "error": "DATABASE_URL missing"
  },
  "events": {
    "warning": "BackOff restarting failed container"
  },
  "deployments": {
    "available_replicas": 0
  },
  "network": {},
  "namespace_scope": {
    "namespace": "payment",
    "affected_workloads": ["payment-api"]
  }
}
```

Expected reasoning:

Root Cause:
Application failed because the DATABASE_URL environment variable is missing.


---

### 4. Fix Recommendation Engine

Generate actionable fixes.

Example:
```
Suggested Fix:

Add the DATABASE_URL environment variable to the deployment configuration and redeploy the application.

kubectl Commands:

kubectl edit deployment payment-api

kubectl rollout restart deployment/payment-api

Prevention Recommendation:

Validate required environment variables during CI/CD deployment and implement startup configuration checks.

```
Recommendations must be:

- practical
- beginner friendly
- Kubernetes-specific

Avoid generic advice.

---

### 5. Confidence Engine

Confidence must be evidence-based.
Do not allow a bad LLM value like 0% to override strong Kubernetes evidence.

Rules:

- ImagePullBackOff / ErrImagePull >= 90%
- CrashLoopBackOff / OOMKilled >= 88%
- FailedPull / BackOff events >= 85%
- Healthy cluster >= 90%
- Weak evidence = lower confidence
- The dashboard should show the final computed confidence score.

---

Example:

```text
Confidence: 92%
```

Reasoning:

```text
High confidence because:

- Pod state = CrashLoopBackOff
- Logs clearly show DATABASE_URL missing
- Kubernetes events show repeated container restarts
- Deployment has 0 available replicas
- All findings indicate application startup failure
```

---


### 6. Incident Severity Engine


Generate severity for every diagnosis.

Supported values:

```text
Critical
High
Medium
Low
```

Default to:

```text
Medium
```

if the LLM does not return a valid severity.

---

Example:

```text
Severity: Critical
```

Reasoning:

```text
Critical because:

- Payment service is unavailable
- Multiple replicas are unhealthy
- Users cannot access the application
```

---

Another Example:

```text
Severity: High
```

Reasoning:

```text
High because:

- Deployment is degraded
- Some replicas are unavailable
- Service is partially impacted
```

The AI should always provide a short explanation for the selected severity level.


---

## FastAPI Integration


Update:

```text id="6iyqrb"
POST /investigate
```

New Flow:

```text id="b1x5mt"
Collect Kubernetes Evidence
        ↓
Send to AI Kubernetes Agent
        ↓
LLM Reasoning
        ↓
Root Cause Analysis
        ↓
Severity Classification
        ↓
Generate kubectl Commands
        ↓
Find Similar Incidents
        ↓
Store Investigation History
        ↓
Generate Investigation Report
        ↓
Return Diagnosis
```

Example API Response:

```json id="jvqbhj"
{
  "status": "success",
  "diagnosis": {
    "root_cause": "DATABASE_URL missing",
    "explanation": "Application cannot connect to the database because the required environment variable is missing.",
    "suggested_fix": "Add the DATABASE_URL environment variable to the deployment.",
    "kubectl_command": "kubectl edit deployment payment-service",
    "confidence": 92,
    "severity": "High",
    "similar_incident_found": true,
    "report_generated": true
  }
}
```

The response should be displayed in the Frontend Diagnosis Dashboard and stored in Investigation History.

---

## Constraints

DO NOT implement:

Authentication
Investigation history
Realtime updates
Frontend changes
Deployment
Only build the AI reasoning layer.

Keep implementation beginner friendly.

Do not overengineer.

DO NOT BREAK EXISTING CODE.

Only extend existing functionality.

---

## Expected Result

When I call:

```http
POST /investigate
```

The system should:

```text
Investigate Kubernetes
        ↓
Reason about failures
        ↓
Find root cause
        ↓
Suggest fix
        ↓
Score confidence and severity
        ↓
Return diagnosis
```

The backend should now behave like:

> A Senior Kubernetes SRE helping troubleshoot incidents.
