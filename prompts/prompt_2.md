# 02-prompt-kubernetes-investigation-layer.md

## Context

Project setup is already completed (Prompt 1).

We now want to build the **Kubernetes Investigation Layer** inside
`investigation-service`.

Architecture:

```text
Frontend Dashboard
    ↓
FastAPI Backend (Orchestrator)
    ↓
Kubernetes Investigation Layer
        ├── Pod Inspector
        ├── Logs Collector
        ├── Events Analyzer
        ├── Deployment Inspector
        ├── Network Inspector
        └── Namespace Scope Analyzer ⭐
    ↓
AI Kubernetes Agent
```

Goal:

This layer should behave like a **junior DevOps engineer collecting
evidence** before AI reasoning starts.

Important:

We are still **NOT implementing AI reasoning**.

We are only collecting Kubernetes troubleshooting data.

Use **kubectl commands internally** (not the Kubernetes Python SDK).

---

## Goal

Goal

Build the Kubernetes Investigation Layer.

Implement:

```
- Pod Inspector
- Logs Collector
- Events Analyzer
- Deployment Inspector
- Network Inspector
- Namespace Scope Analyzer
- Kubectl Executor
- Investigation Service
```

The FastAPI backend should orchestrate these components and produce structured investigation data for the AI Kubernetes Agent.

---

## Requirements

Requirements

### 1. Kubectl Executor

Create a reusable utility to safely execute kubectl commands.

Requirements:

* Use Python subprocess
* Capture stdout/stderr
* Handle failures gracefully
* Return structured output
* Add logging

Example supported commands:

* kubectl get pods -A
* kubectl get events -A
* kubectl logs <pod-name>
* kubectl describe deployment <deployment-name>
* kubectl get svc -A

Keep implementation clean and beginner friendly.

---

### 2. Pod Inspector

Responsibilities:

* Get pod status
* Detect unhealthy pods

Detect:

* CrashLoopBackOff
* ImagePullBackOff
* Pending
* Error
* OOMKilled
* ContainerCreating (stuck)

Return structured JSON.

Example:
```
{
"healthy": false,
"problematic_pods": [
{
"name": "payment-service",
"namespace": "default",
"status": "CrashLoopBackOff"
}
]
}
```
---

### 3. Logs Collector

Responsibilities:

* Fetch logs for failed pods
* Capture relevant failures

Focus on:

* Exceptions
* Connection failures
* Missing environment variables
* Image failures
* Startup errors

Keep logs concise.

Do not return thousands of lines.

---

### 4. Events Analyzer

Responsibilities:

* Read Kubernetes events

Detect:

* FailedScheduling
* BackOff
* FailedMount
* FailedPull
* ErrImagePull
* Unhealthy

Return summarized findings.

---

### 5. Deployment Inspector

Responsibilities:

* Inspect deployments

Check:

* Available replicas
* Unavailable replicas
* Rollout failures
* Deployment conditions

Detect unhealthy deployments.

---

### 6. Network Inspector

Responsibilities:

* Inspect services and networking

Check:

* Service existence
* Selector mismatch
* Missing endpoints
* DNS-related issues

---

### 7. Namespace Scope Analyzer

Responsibilities:

* Analyze target namespace
* Identify affected workloads
* Reduce investigation scope
* Return impacted resources only

Example:

```
{
"namespace": "payment",
"affected_workloads": [
"payment-api",
"payment-worker"
]
}
```
Keep implementation simple and lightweight.

---

### 8. Investigation Service

Create a service that orchestrates everything.

Flow:
```
Check Pods
↓
Collect Logs
↓
Analyze Events
↓
Inspect Deployments
↓
Check Networking
↓
Analyze Namespace Scope
```

Return a single structured investigation payload.

Example:
```
{
"pods": {},
"logs": {},
"events": {},
"deployments": {},
"network": {},
"namespace_scope": {}
}
```
The investigation payload will later be consumed by the AI Kubernetes Agent.


---

## FastAPI API

Create Endpoint:

```text
POST /investigate
```

When called:

1. Run the full investigation flow
2. Return structured Kubernetes evidence

Example response:
```
{
  "status": "success",
  "investigation": {
    "pods": {},
    "logs": {},
    "events": {},
    "deployments": {},
    "network": {},
    "namespace_scope": {}
  }
}
```

No AI yet. 

No root cause analysis yet. 

This step is only evidence gathering.

---

## Constraints

DO NOT implement:

OpenRouter
LLM reasoning
Root cause analysis
Fix recommendation
InsForge integration
Authentication
Realtime updates
Do NOT use Kubernetes SDK.

Use kubectl internally only.

Keep code modular and beginner friendly.

DO NOT BREAK EXISTING CODE.

Only extend the project incrementally.

---

## Expected Result

I should be able to call:

```http
POST /investigate
```

And receive structured Kubernetes troubleshooting evidence 
The backend should now behave like:

> A junior DevOps engineer collecting debugging evidence.
