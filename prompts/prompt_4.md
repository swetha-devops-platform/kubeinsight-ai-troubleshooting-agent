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
Suggested Fix
        ↓
kubectl Remediation Commands
        ↓
Similar Incident Search
        ↓
Investigation Report Generation
````

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
Root Cause Analysis
        ↓
Severity Classification
        ↓
Actionable kubectl Commands
        ↓
Similar Incident Finder
        ↓
Investigation History Storage
        ↓
Investigation Report Generator
```

# Goal:

Build a simple frontend dashboard that allows users to:

```text
Click Investigate
        ↓
Watch Investigation Progress
        ↓
Receive Diagnosis
        ↓
View Similar Incidents
        ↓
Download Investigation Report
        ↓
View Investigation History
```

Keep everything simple and professional.


Use InsForge for:

- Authentication
- Investigation History Storage
- Realtime Investigation Progress Updates

---

## Goal

Build the Frontend Dashboard + API Integration.

Implement:

```text
Minimal Dashboard
Authentication (InsForge)
Realtime Investigation Progress
Diagnosis Display
Investigation History
Report Download
Frontend → Backend Integration
```

Keep UI minimal and clean.

Do not overengineer.

---

## Requirements

### 1. Authentication(Insforge)

Add authentication using InsForge.

Requirements:

* Login support
* Protected dashboard
* User session handling

Only authenticated users can:

```text
Trigger Investigation
View Investigation History
View Diagnosis Results
Download Investigation Reports
```

Keep authentication implementation minimal and clean.

Avoid unnecessary complexity.

---

### 2. Investigation Dashboard

Build a minimal professional dashboard.

UI Sections:

### Header

```text
KubeInsight AI
AI Kubernetes Troubleshooting Agent
```

### Main CTA

Button:

```text
[ Investigate Cluster ]
```

### 3.  Investigation Progress

Show realtime investigation progress.

Example:

```text
✓ Collecting Cluster Information
✓ Checking Pods
✓ Reading Logs
✓ Analyzing Events
✓ Inspecting Deployments
✓ Checking Services
✓ AI Reasoning
✓ Root Cause Identified
✓ Severity Classified
✓ Generating Remediation Commands
✓ Finding Similar Incidents
✓ Investigation Complete
```

Progress should update while the backend investigation runs.

Use InsForge realtime capabilities for progress updates.

Keep the UI simple, clean, and professional.

---

### 4. Diagnosis Card

Display:

```text
Root Cause
Explanation
Severity
Suggested Fix
kubectl Remediation Commands
Confidence Score
```

Example:

```text
Root Cause:
DATABASE_URL missing

Explanation:
Application failed during startup because the required environment variable was not configured.

Severity:
High

Suggested Fix:
Add the missing environment variable to the deployment configuration.

kubectl Command:
kubectl edit deployment payment-service

Confidence:
92%
```

Keep styling clean and beginner friendly.

No complex UI.

Display diagnosis information in a single card after investigation completes.


---

### 5. Similar Incident Section

Display similar past incidents.

Example:

```text
Similar Incidents

CrashLoopBackOff
ImagePullBackOff
OOMKilled
```

Simple list is enough.

---

### 6. Investigation Report

Allow user to download generated report.

Button:

```text
[ Download Report ]
```

Report contains:

```text
Investigation Summary
Evidence Collected
Root Cause
Severity
Suggested Fix
kubectl Commands
Confidence Score
```

---

### 7. Investigation History

Store and display:

```text
Timestamp
Namespace
Root Cause
Severity
Confidence
Status
```

Example:

```text
Recent Investigations

CrashLoopBackOff
ImagePullBackOff
OOMKilled
```

Simple table only.

---

### 8. Frontend API Integration

Frontend should call:

```http
POST /investigate
```

Flow:

```text
User Clicks Investigate
        ↓
Backend Investigation Starts
        ↓
Progress Updates
        ↓
Diagnosis Returned
        ↓
History Stored
        ↓
Report Generated
        ↓
UI Updated
```

Handle:

```text
Loading State
API Errors
Timeouts
Empty Responses
```

---

## UI Layout

```text
------------------------------------------------

KubeInsight AI

[ Investigate Cluster ]

Investigation Progress

✓ Checking Pods
✓ Reading Logs
✓ AI Reasoning

Diagnosis

Root Cause:
CrashLoopBackOff

Severity:
High

Suggested Fix:
Update Environment Variable

Confidence:
92%

Similar Incidents

CrashLoopBackOff
OOMKilled

[ Download Report ]

Recent Investigations

CrashLoopBackOff
ImagePullBackOff

------------------------------------------------
```

Simple.

Professional.

Easy to demonstrate.

---

## Constraints

DO NOT change existing investigation logic.

DO NOT modify AI reasoning workflow.

DO NOT introduce complex frontend architecture.

DO NOT add charts.

DO NOT add unnecessary features.

FastAPI must remain the orchestrator.

Only extend current functionality.

DO NOT BREAK EXISTING CODE.

---

## Expected Result

Users should be able to:

```text
Login
        ↓
Open Dashboard
        ↓
Click Investigate
        ↓
Watch Progress
        ↓
Receive Diagnosis
        ↓
View Similar Incidents
        ↓
Download Report
        ↓
Access Investigation History
```

The application should now feel like:

**KubeInsight AI – An AI-Powered Kubernetes Troubleshooting Platform.**
