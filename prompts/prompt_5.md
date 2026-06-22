# 05-prompt-integration-testing-deployment.md

## Context

The application is now complete.

Users can:

```text
Login
        ↓
Select Kubernetes Cluster
        ↓
Click "Investigate Cluster"
        ↓
Backend investigates Kubernetes
        ↓
Evidence collected
        ↓
AI Kubernetes Agent analyzes findings
        ↓
LLM reasoning performed
        ↓
Root Cause identified
        ↓
Severity classified
        ↓
Remediation commands generated
        ↓
Similar incidents found
        ↓
Investigation report generated
        ↓
History saved
        ↓
Diagnosis displayed
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
Actionable kubectl Remediation Commands
        ↓
Similar Incident Finder
        ↓
Investigation History Storage
        ↓
Investigation Report Generator
        ↓
Frontend Diagnosis Dashboard
```

Goal:

1. Integrate all components end-to-end
2. Test real Kubernetes failures
3. Improve reliability
4. Validate multi-cluster investigation workflow

This step should make the system feel like a real AI-powered Kubernetes troubleshooting platform.

---

## Goal

Implement:

```text
End-to-End Integration
Cluster Selection
Error Handling
Loading States
Real Kubernetes Failure Testing
Investigation Report Validation
```

Keep implementation beginner friendly.

Do not overengineer.

---

## Requirements

### 1. Kubernetes Cluster Selection

Read all available clusters from the user's kubeconfig file.

Display:

```text
Available Clusters

● minikube
● dev-cluster
● staging-cluster
● production-cluster
```

User workflow:

```text
Login
        ↓
Select Cluster
        ↓
Click Investigate
        ↓
Investigation runs against selected cluster
```

Requirements:

* Automatically read kubeconfig
* Display available cluster names
* Allow user to select one cluster
* Pass selected cluster to backend investigation workflow

Keep cluster selection simple.

---

### 2. End-to-End Integration

Validate complete workflow.

Expected flow:

```text
User Selects Cluster
        ↓
User Clicks Investigate
        ↓
Frontend sends API request
        ↓
FastAPI orchestrates investigation
        ↓
Evidence collected
        ↓
AI Kubernetes Agent analyzes evidence
        ↓
Root Cause generated
        ↓
Severity classified
        ↓
Remediation commands generated
        ↓
Similar incidents retrieved
        ↓
Investigation report generated
        ↓
History stored
        ↓
Realtime UI updated
        ↓
User sees diagnosis
```

Ensure all components work together.

Fix integration issues if required.

---

### 3. Improve Reliability

Add proper error handling.

Handle:

```text
kubectl failures
Cluster unreachable
Missing kubeconfig
Invalid cluster selection
OpenRouter failures
API timeout
No unhealthy resources found
Authentication issues
Report generation failures
```

Show user-friendly error messages.

Example:

```text
Unable to connect to the selected Kubernetes cluster.

Please verify:

- kubeconfig configuration
- cluster accessibility
- kubectl permissions
```

Avoid exposing stack traces.

---

### 4. Loading & Empty States

When investigation starts:

```text
Investigating Kubernetes Cluster...
```

During investigation:

```text
✓ Collecting Cluster Information
✓ Checking Pods
✓ Reading Logs
✓ Analyzing Events
✓ Inspecting Deployments
✓ Checking Services
✓ AI Reasoning
✓ Root Cause Analysis
✓ Severity Classification
✓ Generating Remediation Commands
✓ Finding Similar Incidents
✓ Generating Report
```

If no issues are found:

```text
No critical Kubernetes issues detected.

Cluster appears healthy.
```

---

### 5. Test Real Kubernetes Failures

Create intentional Kubernetes failure scenarios.

#### Scenario 1 — CrashLoopBackOff

Example:

```text
Missing environment variable
```

Expected:

```text
Root Cause:
Missing environment variable

Suggested Fix:
Add missing secret/configmap value

Severity:
High
```

---

#### Scenario 2 — ImagePullBackOff

Example:

```text
Wrong image tag
```

Expected:

```text
Root Cause:
Invalid image tag

Suggested Fix:
Update deployment image

Severity:
Medium
```

---

#### Scenario 3 — OOMKilled

Example:

```text
Low memory limits
```

Expected:

```text
Root Cause:
Container exceeded memory limits

Suggested Fix:
Increase memory requests and limits

Severity:
High
```

---

#### Scenario 4 — Service Selector Mismatch

Example:

```text
Incorrect labels
```

Expected:

```text
Root Cause:
Service selector does not match pod labels

Suggested Fix:
Update service selector configuration

Severity:
Medium
```

---

### 6. Diagnosis Validation

Validate that the diagnosis contains:

```text
Root Cause
Explanation
Severity
Suggested Fix
kubectl Remediation Commands
Confidence Score
Similar Incidents
```

Ensure outputs are meaningful and actionable.

---

### 7. Investigation Report Validation

Validate generated report contains:

```text
Investigation Summary
Evidence Collected
Root Cause Analysis
Severity
Suggested Fix
kubectl Commands
Confidence Score
Timestamp
Selected Cluster
```

Report should be downloadable from the dashboard.

---

## Constraints

DO NOT modify existing investigation logic.

DO NOT change AI reasoning workflow.

DO NOT introduce unnecessary complexity.

DO NOT add advanced monitoring features.

FastAPI must remain the orchestrator.

Only improve integration, testing, and reliability.

DO NOT BREAK EXISTING CODE.

---

## Expected Result

Users should be able to:

```text
Login
        ↓
Select Kubernetes Cluster
        ↓
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
Review Investigation History
```

The platform should now feel like:

> KubeInsight AI – A Production-Style AI Kubernetes Troubleshooting Platform.

