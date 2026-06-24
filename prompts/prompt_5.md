# 05-prompt-integration-testing-deployment.md

## Context

The KubeInsight AI platform is now functionally complete.

Implemented components:

```text
Frontend Dashboard
        ↓
Authentication (InsForge)
        ↓
Cluster Selection
        ↓
Namespace Selection
        ↓
Kubernetes Investigation Layer
        ↓
AI Kubernetes Agent
        ↓
Root Cause Analysis
        ↓
Evidence-Based Confidence Scoring
        ↓
Severity Classification
        ↓
kubectl Remediation Commands
        ↓
Similar Incident Finder
        ↓
Investigation History Storage
        ↓
Investigation Report Generator
        ↓
Diagnosis Dashboard
```

The application should now function as a complete end-to-end AI-powered Kubernetes troubleshooting platform.

---

# Goal

Implement:

```text
End-to-End Integration

Reliability Improvements

Cluster Connectivity Validation

Kubernetes Failure Testing

Investigation Validation

Report Validation

History Validation

Deployment Readiness
```

Keep implementation beginner friendly.

Do not overengineer.

---

# Current Architecture

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
kubectl Remediation Commands
        ↓
Similar Incident Finder
        ↓
Investigation History Storage
        ↓
Investigation Report Generator
        ↓
Frontend Diagnosis Dashboard
```

---

# 1. Cluster Selection Validation

Read available clusters from kubeconfig.

Display:

```text
Available Clusters

● minikube
● dev-cluster
● staging-cluster
● production-cluster
```

Requirements:

* Automatically discover kubeconfig contexts
* Populate cluster dropdown
* Support manual cluster entry
* Refresh cluster list
* Pass selected cluster into investigation workflow

Selected cluster must appear in:

```text
Investigation History

Generated Reports

Diagnosis Results
```

---

# 2. Namespace Selection Validation

Support:

```text
default

kube-system

monitoring

all

custom namespace
```

Requirements:

* Namespace filtering
* Namespace-aware investigation
* Namespace displayed in reports
* Namespace stored in history

---

# 3. Kubernetes Connectivity Validation

Before investigation starts:

Validate:

```bash
kubectl cluster-info

kubectl get nodes

kubectl get pods -A
```

If validation fails:

Display:

```text
Unable to connect to the selected Kubernetes cluster.

Please verify:

- kubeconfig configuration
- cluster accessibility
- kubectl permissions
- active cluster status
```

Do not continue investigation if connectivity validation fails.

---

# 4. End-to-End Integration Validation

Validate complete workflow.

Expected flow:

```text
User Selects Cluster
        ↓
User Selects Namespace
        ↓
User Clicks Investigate
        ↓
Evidence Collection
        ↓
AI Analysis
        ↓
Root Cause Analysis
        ↓
Confidence Calculation
        ↓
Severity Classification
        ↓
kubectl Commands Generated
        ↓
Similar Incident Search
        ↓
Investigation Stored
        ↓
Report Generated
        ↓
Dashboard Updated
```

All components must work together.

---

# 5. Reliability Improvements

Handle failures gracefully.

Support:

```text
kubectl failures

Cluster unreachable

Missing kubeconfig

Invalid cluster

OpenRouter failures

API timeouts

LLM malformed responses

Authentication failures

Report generation failures

History storage failures
```

Display user-friendly messages.

Never expose stack traces.

---

# 6. Investigation Progress Validation

Display:

```text
✓ Validating Cluster Access

✓ Collecting Cluster Information

✓ Checking Pods

✓ Reading Logs

✓ Analyzing Events

✓ Inspecting Deployments

✓ Inspecting Networking

✓ AI Reasoning

✓ Root Cause Analysis

✓ Severity Classification

✓ Confidence Calculation

✓ Generating kubectl Commands

✓ Finding Similar Incidents

✓ Saving Investigation

✓ Generating Report

✓ Investigation Complete
```

Progress updates should appear in realtime.

---

# 7. Kubernetes Failure Testing

Validate using real failures.

---

## Scenario 1 — CrashLoopBackOff

Create:

```bash
kubectl create deployment crashloop-demo \
--image=busybox \
-- /bin/sh -c "exit 1"
```

Expected:

```text
Root Cause:
Application repeatedly crashing

Severity:
HIGH

Confidence:
88%+
```

---

## Scenario 2 — ImagePullBackOff

Create:

```bash
kubectl create deployment imagepullbackoff-demo \
--image=nginx:invalidtag123
```

Expected:

```text
Root Cause:
Invalid image tag

Severity:
MEDIUM

Confidence:
90%+
```

---

## Scenario 3 — OOMKilled

Expected:

```text
Root Cause:
Container exceeded memory limit

Severity:
HIGH

Confidence:
88%+
```

---

## Scenario 4 — FailedScheduling

Expected:

```text
Root Cause:
Insufficient cluster resources

Severity:
MEDIUM

Confidence:
85%+
```

---

## Scenario 5 — Service Selector Mismatch

Expected:

```text
Root Cause:
Service selector mismatch

Severity:
MEDIUM

Confidence:
85%+
```

---

# 8. Diagnosis Validation

Verify every diagnosis contains:

```text
Root Cause

Explanation

Suggested Fix

kubectl Commands

Prevention Recommendation

Confidence Score

Confidence Explanation

Severity

Severity Explanation
```

Also include:

```text
Analysis Source
```

Values:

```text
AI

Fallback Rule Engine
```

---

# 9. Similar Incident Validation

Verify:

```text
Similar Incidents
```

returns up to two matching historical incidents.

Validate:

* Similar Root Cause
* Similar Severity
* Similar Failure Pattern

---

# 10. Investigation History Validation

Verify storage of:

```text
Timestamp

Cluster

Namespace

Root Cause

Severity

Confidence

Status
```

Validate:

```text
Recent Investigations Table

Clear History Button
```

Requirements:

* History clears immediately
* UI refreshes automatically
* No page reload required

---

# 11. Investigation Report Validation

Verify downloadable reports contain:

```text
Timestamp

Cluster

Namespace

Evidence Collected

Root Cause

Explanation

Severity

Confidence

Suggested Fix

kubectl Commands

Prevention Recommendation
```

Report download must work from dashboard.

---

# 12. Dashboard Validation

Verify dashboard contains:

```text
Cluster Selector

Namespace Input

Investigation Button

Progress Checklist

Diagnosis Card

Severity Card

Confidence Card

Source Card

Similar Incidents

Recent Investigations

Download Report

Clear History
```

Design:

```text
Dark Theme

Professional SRE Style

Responsive Layout
```

---

# Constraints

DO NOT modify:

```text
Investigation Logic

AI Reasoning Logic

Confidence Rules

Severity Rules
```

FastAPI remains the orchestrator.

Only improve integration, validation, deployment readiness, and reliability.

Do not break existing code.

---

# Expected Result

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
Review Investigation History
        ↓
Clear History
```

The platform should now feel like:

> KubeInsight AI – A Production-Style AI-Powered Kubernetes Troubleshooting Platform.
