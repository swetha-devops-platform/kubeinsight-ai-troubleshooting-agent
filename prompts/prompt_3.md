# 03-prompt-ai-reasoning-engine.md

## Context

The Kubernetes Investigation Layer is complete (Prompt 2).

The system can now collect Kubernetes troubleshooting evidence.

Collected evidence includes:

```text
Cluster
Namespace
Pods
Logs
Events
Deployments
Services
Endpoints
Networking Findings
Namespace Scope
Investigation Summary
```

Architecture:

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
Root Cause Analysis
        ↓
Severity Classification
        ↓
Evidence-Based Confidence Scoring
        ↓
Actionable kubectl Commands
        ↓
Diagnosis Object
```

Goal:

The AI Kubernetes Agent should behave like a:

```text
Senior Kubernetes SRE
```

The agent should:

* Analyze Kubernetes evidence
* Correlate logs, events, deployments, networking, and namespace scope
* Determine root cause
* Suggest Kubernetes-specific fixes
* Generate remediation commands
* Classify severity
* Calculate confidence score
* Provide prevention recommendations

Important:

Use OpenRouter credentials provided via InsForge.

Never hardcode secrets.

---

# Goal

Build the AI Kubernetes Agent.

Implement:

```text
Prompt Builder

LLM Client

Root Cause Analyzer

Fix Recommendation Engine

Confidence Engine

Severity Engine
```

The AI layer consumes the investigation payload generated in Prompt 2.

---

# Project Structure

```text
investigation-service/

└── app
    ├── ai
    │   ├── prompt_builder.py
    │   ├── llm_client.py
    │   ├── root_cause_analyzer.py
    │   ├── confidence_engine.py
    │   ├── severity_engine.py
    │   └── diagnosis_service.py
```

Keep implementation simple and beginner friendly.

---

# 1. Prompt Builder

Create:

```text
prompt_builder.py
```

The system prompt should make the model behave like:

```text
Senior Kubernetes SRE
```

Prompt must include:

```text
Cluster
Namespace
Affected Workloads
Pod Status
Logs
Events
Deployment Health
Networking Findings
Namespace Scope
```

The model must return:

```text
1. Root Cause

2. Explanation

3. Suggested Fix

4. kubectl Commands

5. Prevention Recommendation

6. Severity

7. Severity Explanation
```

Prompts should be structured and deterministic.

Avoid vague responses.

---

# 2. LLM Client

Create:

```text
llm_client.py
```

Use:

```text
OpenRouter
```

Read from:

```env
OPENROUTER_API_KEY=
OPENROUTER_MODEL=
```

Requirements:

* HTTPX
* Timeout handling
* Retry support
* Error handling
* Structured response parsing
* Clean logging

Never expose secrets.

---

# 3. Root Cause Analyzer

Create:

```text
root_cause_analyzer.py
```

Consume the investigation payload from Prompt 2.

Example input:

```json
{
  "pods": {
    "status": "CrashLoopBackOff"
  },
  "logs": {
    "error": "DATABASE_URL missing"
  },
  "events": {
    "warning": "BackOff restarting failed container"
  }
}
```

Expected output:

```text
Root Cause:

Application failed because DATABASE_URL is missing.
```

The analyzer should correlate:

* Pod state
* Logs
* Events
* Deployment health
* Networking findings

---

# 4. Fix Recommendation Engine

Generate actionable fixes.

Example:

```text
Suggested Fix:

Add DATABASE_URL to the deployment configuration.

kubectl Commands:

kubectl edit deployment payment-api

kubectl rollout restart deployment/payment-api

Prevention Recommendation:

Validate required environment variables during CI/CD deployment.
```

Recommendations must be:

* Practical
* Beginner friendly
* Kubernetes specific

Avoid generic advice.

---

# 5. Confidence Engine

Create:

```text
confidence_engine.py
```

Confidence must be evidence-based.

Do NOT allow weak LLM output to override strong Kubernetes evidence.

Rules:

```text
ImagePullBackOff >= 90%

ErrImagePull >= 90%

CrashLoopBackOff >= 88%

OOMKilled >= 88%

FailedScheduling >= 85%

BackOff Events >= 85%

Healthy Cluster >= 90%

Weak Evidence = Lower Confidence
```

Example:

```text
Confidence: 92%
```

Explanation:

```text
CrashLoopBackOff detected

BackOff events detected

Logs show missing DATABASE_URL

Deployment has zero available replicas
```

Return:

```json
{
  "confidence": 92,
  "confidence_explanation": ""
}
```

---

# 6. Severity Engine

Create:

```text
severity_engine.py
```

Supported values:

```text
Critical

High

Medium

Low
```

Default:

```text
Medium
```

if no valid severity is produced.

Example:

```text
Severity: Critical
```

Explanation:

```text
Payment service unavailable

All replicas unhealthy

User traffic impacted
```

Another example:

```text
Severity: High
```

Explanation:

```text
Deployment degraded

Partial service impact
```

Return:

```json
{
  "severity": "HIGH",
  "severity_explanation": ""
}
```

---

# 7. Diagnosis Service

Create:

```text
diagnosis_service.py
```

Responsibilities:

```text
Receive Investigation Payload
        ↓
Build Prompt
        ↓
Call OpenRouter
        ↓
Generate Diagnosis
        ↓
Calculate Confidence
        ↓
Calculate Severity
        ↓
Return Structured Result
```

---

# FastAPI Integration

Update:

```text
POST /investigate
```

Flow:

```text
Collect Kubernetes Evidence
        ↓
Send To AI Kubernetes Agent
        ↓
LLM Reasoning
        ↓
Root Cause Analysis
        ↓
Confidence Scoring
        ↓
Severity Classification
        ↓
Generate kubectl Commands
        ↓
Return Diagnosis
```

Response:

```json
{
  "status": "success",
  "diagnosis": {
    "root_cause": "",
    "explanation": "",
    "suggested_fix": "",
    "kubectl_commands": [],
    "prevention_recommendation": "",
    "confidence": 0,
    "confidence_explanation": "",
    "severity": "",
    "severity_explanation": "",
    "analysis_source": ""
  }
}
```

analysis_source values:

```text
AI

Fallback Rule Engine
```

---

# Constraints

DO NOT implement:

```text
Authentication

Investigation History

Similar Incident Finder

Report Generator

Realtime Updates

Frontend Changes
```

Only build the AI reasoning layer.

Do not break existing code.

Keep implementation modular and beginner friendly.

---

# Expected Result

When I call:

```http
POST /investigate
```

The system should:

```text
Collect Evidence
        ↓
Analyze Kubernetes Failures
        ↓
Determine Root Cause
        ↓
Generate Fixes
        ↓
Generate kubectl Commands
        ↓
Calculate Confidence
        ↓
Classify Severity
        ↓
Return Diagnosis
```

The backend should now behave like:

> A Senior Kubernetes SRE helping troubleshoot Kubernetes incidents.
