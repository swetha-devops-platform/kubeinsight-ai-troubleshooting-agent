# 04-prompt-dashboard-and-api.md

## Context

The backend can now perform:

```text
Investigate Kubernetes
        ↓
Collect Evidence
        ↓
AI Reasoning
        ↓
Root Cause Analysis
        ↓
Severity Classification
        ↓
Evidence-Based Confidence Scoring
        ↓
Suggested Fix
        ↓
kubectl Remediation Commands
        ↓
Similar Incident Search
        ↓
Investigation History Storage
        ↓
Investigation Report Generation
```

---

## Architecture

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
Confidence Scoring
        ↓
Actionable kubectl Commands
        ↓
Similar Incident Finder
        ↓
Investigation History Storage
        ↓
Investigation Report Generator
```

---

## Goal

Build the KubeInsight AI Dashboard.

Users should be able to:

```text
Login
      ↓
Select Kubernetes Cluster
      ↓
Select Namespace
      ↓
Run Investigation
      ↓
Watch Investigation Progress
      ↓
Receive Diagnosis
      ↓
View Confidence Score
      ↓
View Severity
      ↓
View Similar Incidents
      ↓
Download Investigation Report
      ↓
Review Investigation History
```

---

## Authentication

Use InsForge.

Requirements:

* Login
* Signup
* Session Handling
* Protected Dashboard
* Logout

Only authenticated users can:

* Run Investigations
* Access Investigation History
* Download Reports
* View Similar Incidents

---

## Dashboard Layout

### Header

```text
KubeInsight AI
AI-Powered Kubernetes Troubleshooting Agent
```

---

### Cluster Selection

Display available clusters.

Example:

```text
Cluster

[ minikube ▼ ]

[ Refresh ]
```

Support manual cluster entry if auto-discovery fails.

---

### Namespace Selection

Allow namespace filtering.

Example:

```text
Namespace

[ default ]
```

Support:

```text
default
kube-system
monitoring
all
custom namespace
```

---

### Investigation Action

Primary button:

```text
[ Investigate Cluster ]
```

Disable while investigation is running.

---

## Investigation Progress

Show realtime progress.

Example:

```text
✓ Collecting Cluster Information
✓ Checking Pods
✓ Reading Logs
✓ Analyzing Events
✓ Inspecting Deployments
✓ Inspecting Services
✓ Inspecting Networking
✓ AI Reasoning
✓ Root Cause Identified
✓ Severity Classified
✓ Calculating Confidence
✓ Generating Commands
✓ Finding Similar Incidents
✓ Generating Report
✓ Investigation Complete
```

Use InsForge realtime updates.

---

## Diagnosis Panel

Display investigation results.

Fields:

```text
Root Cause

Explanation

Suggested Fix

kubectl Commands

Prevention Recommendation
```

---

## Severity Card

Display:

```text
Severity

CRITICAL
HIGH
MEDIUM
LOW
```

Show severity explanation.

---

## Confidence Card

Display:

```text
Confidence Score

92%
```

Also show:

```text
Confidence Explanation
```

Example:

```text
CrashLoopBackOff detected

BackOff events detected

Container restart count high

Logs confirm startup failure
```

---

## Analysis Source Card

Display source:

```text
AI Analysis

or

Fallback Rule Engine
```

Useful when LLM is unavailable.

---

## Similar Incidents

Display:

```text
Similar Incidents

CrashLoopBackOff

ImagePullBackOff

OOMKilled
```

Show up to two similar incidents.

---

## Investigation History

Display:

```text
Recent Investigations
```

Columns:

```text
Timestamp

Cluster

Namespace

Root Cause

Severity

Confidence

Status
```

Include:

```text
[ Clear History ]
```

Requirements:

* Delete stored history
* Refresh UI immediately
* No page reload

---

## Report Download

Display:

```text
[ Download Report ]
```

Report contains:

* Timestamp
* Cluster
* Namespace
* Root Cause
* Explanation
* Severity
* Confidence
* Suggested Fix
* kubectl Commands
* Prevention Recommendation
* Collected Evidence

---

## API Integration

Frontend should call:

```http
POST /investigate
```

Payload:

```json
{
  "cluster": "minikube",
  "namespace": "default"
}
```

Flow:

```text
User Clicks Investigate
        ↓
Backend Starts Investigation
        ↓
Realtime Progress Updates
        ↓
Diagnosis Returned
        ↓
History Saved
        ↓
Report Generated
        ↓
UI Updated
```

---

## Error Handling

Handle:

* Backend Offline
* Investigation Failure
* Empty Responses
* LLM Failure
* Kubernetes Access Failure
* RBAC Errors
* kubeconfig Errors
* Timeout Errors

Show meaningful messages.

Never display generic:

```text
Something went wrong
```

---

## UI Design Requirements

Use:

* Next.js
* TypeScript
* TailwindCSS

Design:

* Dark Theme
* Professional SRE Style
* Responsive Layout
* Clean Cards
* Minimal Design

Do NOT add:

* Complex Charts
* Fancy Animations
* Overengineered Components

---

## Constraints

DO NOT modify:

* Investigation Layer
* AI Reasoning Logic
* Confidence Logic
* Severity Logic

FastAPI remains the orchestrator.

Only build dashboard and integration.

---

## Expected Result

Users should be able to:

```text
Login
      ↓
Select Cluster
      ↓
Select Namespace
      ↓
Investigate
      ↓
Watch Progress
      ↓
Receive Diagnosis
      ↓
View Confidence
      ↓
View Severity
      ↓
View Similar Incidents
      ↓
Download Report
      ↓
View Investigation History
      ↓
Clear History
```

The application should now feel like:

"KubeInsight AI – An AI-Powered Kubernetes Troubleshooting Platform."
