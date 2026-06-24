# Prerequisites

Before running KubeInsight AI, ensure the following components are configured.

## 1. Kubernetes Cluster Setup

Set up a Kubernetes cluster for investigation.

Supported:

- Minikube (Recommended)
- Kind
- Existing Kubernetes Cluster

Verify cluster access:

```bash
kubectl cluster-info

kubectl get nodes

kubectl get pods -A
```

---

<img width="1427" height="393" alt="1" src="https://github.com/user-attachments/assets/2b8c1670-4e40-45bd-8693-f358e369e0a1" />


---

## 2. kubectl Configuration

Ensure kubectl is installed and configured.

Verify:

```bash
kubectl version --client

kubectl config get-contexts
```

---

<img width="1258" height="326" alt="2" src="https://github.com/user-attachments/assets/fd0895da-d97d-4fed-a314-41abf61c289b" />

---

## 3. Docker Setup

Install:

* Docker
* Docker Compose

Verify:

```bash
docker --version

docker compose version
```

---

## 4. InsForge Setup

Create an InsForge project.

Configure:

* Authentication
* Session Management
* Investigation History Storage
* Realtime Progress Updates

Add the following variables:

```env
INSFORGE_URL=
INSFORGE_ANON_KEY=
INSFORGE_PROJECT_ID=
```

---

<img width="1917" height="799" alt="3" src="https://github.com/user-attachments/assets/f965ecf1-97a4-4048-8014-35856ebb1113" />


---

## 5. Environment Configuration

Create:

```text
.env
```

Configure all required values:

```env
OPENROUTER_API_KEY=
OPENROUTER_MODEL=

INSFORGE_URL=
INSFORGE_ANON_KEY=
INSFORGE_PROJECT_ID=
```

---

## 7. Clone Repository

```bash
git clone <repository-url>

cd kubeinsight-ai
```

---

## 8. Start Application

```bash
docker compose up --build
```

Frontend:

```text
http://localhost:3000
```

Backend:

```text
http://localhost:8000
```

---


<img width="1740" height="660" alt="image" src="https://github.com/user-attachments/assets/3cf5036e-886f-44f2-9a25-5555aafc5392" />


---

This is the same style used in many professional GitHub portfolio projects.
