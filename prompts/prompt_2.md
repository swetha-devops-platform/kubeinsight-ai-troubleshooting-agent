# 02-prompt-kubernetes-investigation-layer.md

## Context

Project setup is already completed (Prompt 1).

We now want to build the Kubernetes Investigation Layer inside the `investigation-service`.

Architecture:

```text
Frontend Dashboard
        ↓
FastAPI Backend (Orchestrator)
        ↓
Kubernetes Investigation Layer
        ├── Kubectl Executor
        ├── Pod Inspector
        ├── Logs Collector
        ├── Events Analyzer
        ├── Deployment Inspector
        ├── Network Inspector
        └── Namespace Scope Analyzer
        ↓
AI Kubernetes Agent
```

Goal:

This layer should behave like a **junior DevOps engineer collecting evidence** before AI reasoning starts.

Important:

We are still NOT implementing:

* OpenRouter
* LLM reasoning
* Root cause analysis
* Severity classification
* Confidence scoring
* Suggested fixes
* kubectl remediation commands
* Similar Incident Finder
* Investigation History
* Report Generator

This prompt is ONLY for Kubernetes evidence collection.

Use:

```text
kubectl
```

internally for all Kubernetes interactions.

Do NOT use the Kubernetes Python SDK.

---

# Goal

Build the Kubernetes Investigation Layer.

Implement:

```text
Kubectl Executor

Pod Inspector

Logs Collector

Events Analyzer

Deployment Inspector

Network Inspector

Namespace Scope Analyzer

Investigation Service
```

The FastAPI backend should orchestrate these components and produce structured investigation data for the AI Kubernetes Agent.

---

# Project Structure

```text
investigation-service/

└── app
    ├── api
    │   └── investigate.py
    │
    ├── investigation
    │   ├── kubectl_executor.py
    │   ├── pod_inspector.py
    │   ├── logs_collector.py
    │   ├── events_analyzer.py
    │   ├── deployment_inspector.py
    │   ├── network_inspector.py
    │   ├── namespace_scope.py
    │   └── investigation_service.py
    │
    └── main.py
```

Keep the implementation modular and beginner friendly.

---

# 1. Kubectl Executor

Create:

```text
kubectl_executor.py
```

Responsibilities:

* Execute kubectl commands safely
* Use Python subprocess
* Capture stdout
* Capture stderr
* Handle failures gracefully
* Return structured output
* Add logging

Example supported commands:

```bash
kubectl get pods -A

kubectl get events -A

kubectl get deployments -A

kubectl get svc -A

kubectl get endpoints -A

kubectl logs <pod-name>

kubectl describe deployment <deployment-name>
```

Example response:

```json
{
  "success": true,
  "output": "...",
  "error": null
}
```

Do not crash if kubectl fails.

---

# 2. Pod Inspector

Create:

```text
pod_inspector.py
```

Responsibilities:

* Retrieve pod status
* Detect unhealthy workloads

Detect:

```text
CrashLoopBackOff

ImagePullBackOff

ErrImagePull

Pending

Error

OOMKilled

ContainerCreating

FailedScheduling
```

Return:

```json
{
  "healthy": false,
  "problematic_pods": [
    {
      "name": "payment-api",
      "namespace": "payment",
      "status": "CrashLoopBackOff"
    }
  ]
}
```

---

# 3. Logs Collector

Create:

```text
logs_collector.py
```

Responsibilities:

* Fetch logs for problematic pods
* Keep logs concise

Capture:

```text
Exceptions

Connection failures

Startup failures

Database errors

Missing environment variables

Application crashes
```

Limit returned logs.

Do not return thousands of lines.

---

# 4. Events Analyzer

Create:

```text
events_analyzer.py
```

Responsibilities:

* Read Kubernetes events
* Summarize relevant findings

Detect:

```text
FailedScheduling

BackOff

FailedMount

FailedPull

ErrImagePull

Unhealthy
```

Return structured summaries.

Example:

```json
{
  "issues": [
    {
      "type": "FailedScheduling",
      "message": "0/1 nodes available"
    }
  ]
}
```

---

# 5. Deployment Inspector

Create:

```text
deployment_inspector.py
```

Responsibilities:

Inspect deployments.

Check:

```text
Desired Replicas

Available Replicas

Unavailable Replicas

Deployment Conditions

Rollout Failures
```

Detect unhealthy deployments.

Return deployment health information.

---

# 6. Network Inspector

Create:

```text
network_inspector.py
```

Responsibilities:

Inspect Kubernetes networking.

Check:

```text
Services

Endpoints

Selector Matching

Missing Endpoints

DNS-related Issues
```

Detect:

```text
Service Selector Mismatch

Missing Service Endpoints
```

Return structured findings.

---

# 7. Namespace Scope Analyzer

Create:

```text
namespace_scope.py
```

Responsibilities:

* Analyze target namespace
* Identify affected workloads
* Reduce investigation scope

Example:

```json
{
  "namespace": "payment",
  "affected_workloads": [
    "payment-api",
    "payment-worker"
  ]
}
```

Keep implementation lightweight.

---

# 8. Investigation Service

Create:

```text
investigation_service.py
```

Responsibilities:

Orchestrate all investigation modules.

Flow:

```text
Check Pods
      ↓
Collect Logs
      ↓
Analyze Events
      ↓
Inspect Deployments
      ↓
Inspect Networking
      ↓
Analyze Namespace Scope
```

Return a single structured investigation payload.

Example:

```json
{
  "cluster": "minikube",
  "namespace": "default",
  "pods": {},
  "logs": {},
  "events": {},
  "deployments": {},
  "network": {},
  "namespace_scope": {},
  "summary": {
    "problematic_pods": 2,
    "unhealthy_deployments": 1,
    "network_issues": 1
  }
}
```

This payload will later be consumed by the AI Kubernetes Agent.

---

# FastAPI API

Create Endpoint:

```text
POST /investigate
```

Request:

```json
{
  "cluster": "minikube",
  "namespace": "default"
}
```

When called:

1. Run the full investigation flow
2. Return structured Kubernetes evidence

Response:

```json
{
  "status": "success",
  "investigation": {
    "cluster": "minikube",
    "namespace": "default",
    "pods": {},
    "logs": {},
    "events": {},
    "deployments": {},
    "network": {},
    "namespace_scope": {},
    "summary": {}
  }
}
```

No AI yet.

No root cause analysis.

Evidence gathering only.

---

# Kubernetes Access Requirements

The backend container must use:

```text
kubectl
```

to access Minikube.

Support:

```yaml
volumes:
  - ${HOME}/.kube:/root/.kube:ro
  - ${HOME}/.minikube:${HOME}/.minikube:ro
```

The investigation layer should work with:

```bash
kubectl get pods -A

kubectl get events -A

kubectl get deployments -A

kubectl get svc -A

kubectl get endpoints -A
```

---

# Constraints

DO NOT implement:

```text
OpenRouter

LLM Reasoning

Root Cause Analysis

Suggested Fixes

Severity Classification

Confidence Scoring

kubectl Remediation Commands

Investigation History

Similar Incident Finder

Report Generator

InsForge Authentication

Realtime Updates
```

Do NOT use:

```text
Kubernetes Python SDK
```

Use:

```text
kubectl only
```

Keep code modular.

Keep code beginner friendly.

Do not break existing code.

Only extend the project incrementally.

---

# Expected Result

I should be able to call:

```http
POST /investigate
```

and receive structured Kubernetes troubleshooting evidence.

The backend should now behave like:

> A junior DevOps engineer collecting debugging evidence before AI analysis begins.
