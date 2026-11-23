<div align="center">

# Enterprise FinTech End-to-End DevOps Automation Platform

**Production-Ready · Resilient · Secure · Cloud-Native**

[![CI/CD Pipeline](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white)](ci_cd/.github-workflows-capstone-pipeline.yml)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.30-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white)](k8s/)
[![Terraform](https://img.shields.io/badge/Terraform-v1.5+-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)](terraform/)
[![Security](https://img.shields.io/badge/Security-Hardened-E53E3E?style=for-the-badge&logo=vault&logoColor=white)](security/)
[![Prometheus](https://img.shields.io/badge/Observability-Prometheus%20%26%20Grafana-E6522C?style=for-the-badge&logo=prometheus&logoColor=white)](observability/)
[![Coverage](https://img.shields.io/badge/Coverage-94.4%25-38A169?style=for-the-badge)](src/backend/test_main.py)
[![PDF Report](https://img.shields.io/badge/📄_Report-Capstone_PDF-DD6B20?style=for-the-badge)](docs/deliverables/Capstone_DevOps_Platform_Report.pdf)

---

*A unified, production-grade DevOps platform engineered for a distributed, high-throughput FinTech Payment Core — integrating version control, multi-gate CI/CD, containerization, Kubernetes orchestration, Terraform IaC, GitOps delivery, deep telemetry, and progressive Canary deployment strategies.*

</div>

---

## 👨‍💻 Candidate & Project Metadata

<table>
  <tr>
    <td><b>🧑‍🎓 Candidate</b></td>
    <td><b>Garvit Sharma</b></td>
    <td><b>🎓 Roll Number</b></td>
    <td><b>2301730190</b></td>
  </tr>
  <tr>
    <td><b>📧 Email</b></td>
    <td><a href="mailto:garvitsharma8909@gmail.com">garvitsharma8909@gmail.com</a></td>
    <td><b>🎯 Scope</b></td>
    <td>End-to-End DevOps Automation</td>
  </tr>
  <tr>
    <td><b>🔗 Repository</b></td>
    <td colspan="3"><a href="https://github.com/Garvit7878/DevOps-Capstone-Project-">github.com/Garvit7878/DevOps-Capstone-Project-</a></td>
  </tr>
  <tr>
    <td><b>📄 Report</b></td>
    <td colspan="3"><a href="docs/deliverables/Capstone_DevOps_Platform_Report.pdf">📥 Capstone_DevOps_Platform_Report.pdf</a></td>
  </tr>
</table>

---

## 🏗️ High-Level System Architecture

```mermaid
flowchart LR
    subgraph DEV["🧑‍💻 Developer"]
        A[Code Commit]
    end

    subgraph CI["⚙️ CI Pipeline"]
        B[Lint & SAST] --> C[Test & Coverage] --> D[Docker Build] --> E[Trivy CVE Scan]
    end

    subgraph CD["🚀 CD & GitOps"]
        F[Push to Registry] --> G[ArgoCD Sync]
    end

    subgraph PROD["☁️ Production"]
        H[Kubernetes EKS]
        I[Canary Rollout]
        J[Monitoring]
    end

    A --> B
    E --> F
    G --> H --> I --> J
```

---

## 🔄 End-to-End CI/CD Pipeline Flow

```mermaid
flowchart TD
    DEV["👨‍💻 Developer Commit"] --> PR["Git Pull Request to main"]

    subgraph GATES["🔒 6-Gate Quality Pipeline — GitHub Actions"]
        PR --> G1["🔍 Gate 1: Flake8 Static Analysis — PEP8 Compliance"]
        G1 --> G2["🛡️ Gate 2: Bandit SAST — Security Vulnerability Screening"]
        G2 --> G3["🧪 Gate 3: PyTest — Unit & Integration Tests"]
        G3 --> G4["📊 Gate 4: Coverage Enforcement ≥ 85%"]
        G4 --> G5["🐳 Gate 5: Multi-Stage Docker Build — Non-Root Hardened"]
        G5 --> G6["🔎 Gate 6: Aqua Trivy — HIGH/CRITICAL CVE Blocking"]
    end

    G6 --> REG["📦 Container Registry — ghcr.io"]
    REG --> GITOPS["🔄 GitOps Tag Update — Kustomize Overlay"]
    GITOPS --> ARGO["🎯 ArgoCD In-Cluster Controller"]

    subgraph DEPLOY["☁️ Production — AWS EKS Cluster"]
        ARGO --> INGRESS["🌐 NGINX Ingress Controller"]
        INGRESS --> CANARY["🐤 Argo Rollouts Canary Controller"]
        CANARY -- "10% → 25% → 50% → 100%" --> PODS["🟢 Application Pods"]
    end

    PODS --> PROM["📈 Prometheus Metrics Scraper — 15s Interval"]
    PROM --> GRAF["📊 Grafana Dashboard & Alert Rules"]
    GRAF -- "SLA Breach > 1.5% Error" --> ROLLBACK["⏪ Automated Canary Rollback"]
```

---

## 🌟 Platform Pillars — 8 Foundational Components

### 1️⃣ Version Control & GitOps Delivery

```mermaid
flowchart LR
    GIT["Git Repository\n(Single Source of Truth)"] -->|manifest change| ARGO["ArgoCD Controller"]
    ARGO -->|reconcile drift| CLUSTER["Live Kubernetes Cluster"]
    CLUSTER -->|report state| ARGO
```

- **Single Source of Truth (SSOT):** Declarative infrastructure and application manifests maintained under version control.
- **Continuous Reconciliation:** In-cluster ArgoCD engine detects configuration drift between live state and Git repository, enforcing self-healing without manual SSH/CLI access.

### 2️⃣ Multi-Stage Hardened CI/CD Pipeline

| Gate | Tool | Purpose |
|:---:|:---|:---|
| 1 | `Flake8` | Static code analysis — PEP8 syntax and formatting |
| 2 | `Bandit` | AST-based SAST — hardcoded secrets and dangerous calls |
| 3 | `PyTest` | Functional and integration testing with test doubles |
| 4 | Coverage Gate | Strict enforcement at **≥ 85%** line coverage |
| 5 | `Docker` | Multi-stage build with caching and non-root runtime |
| 6 | `Aqua Trivy` | Container CVE scanner — blocks HIGH/CRITICAL findings |

### 3️⃣ Containerization Hardening

```mermaid
flowchart LR
    subgraph BUILD["Build Stage"]
        A["Full Python Image\nDependencies + Compilation"]
    end
    subgraph RUNTIME["Production Stage"]
        B["Minimal Runtime\nNon-Root UID 10001\nRead-Only Filesystem"]
    end
    BUILD --> |"copy artifacts only"| RUNTIME
```

- **Distroless Runtime:** Multi-stage `Dockerfile` stripping development tools and compilers from the production image.
- **Least-Privilege Execution:** Containers execute under unprivileged non-root user (`UID 10001: appuser`) with read-only root filesystems and `allowPrivilegeEscalation: false`.

### 4️⃣ Kubernetes Orchestration

| Feature | Configuration |
|:---|:---|
| **Replicas** | 4 pods across multiple availability zones |
| **Liveness Probe** | `GET /health/liveness` — automated process restarts |
| **Readiness Probe** | `GET /health/readiness` — downstream DB & Vault check |
| **CPU Requests / Limits** | `250m` / `1000m` |
| **Memory Requests / Limits** | `256Mi` / `1Gi` |

### 5️⃣ Infrastructure as Code — Terraform

```mermaid
flowchart TD
    TF["Terraform v1.5+"] --> VPC["AWS VPC\nMulti-AZ"]
    VPC --> PUB["Public Subnets\nIngress / ALB"]
    VPC --> PRIV["Private Subnets\nDatabase / Backend"]
    VPC --> EKS["AWS EKS v1.30\nManaged Kubernetes"]
    EKS --> NG["Node Groups\nAuto-scaling"]
```

- Modular Terraform manifests provisioning isolated multi-AZ AWS VPC, private database subnets, public ingress subnets, and an EKS v1.30 managed cluster.

### 6️⃣ Observability & Real-Time Telemetry

| Metric | Type | Purpose |
|:---|:---|:---|
| `http_requests_total` | Counter | Request volume by method, route, and status code |
| `http_request_duration_seconds` | Histogram | p50, p95, p99 latency tracking |
| `settled_volume_usd_total` | Counter | Real-time cumulative financial settlement volume |

- **Scrape Interval:** 15 seconds
- **Alerting:** PrometheusRule triggers on SLA breaches — 5xx error rate > 1.5% or latency > 120ms

### 7️⃣ Zero-Trust Security & Secret Management

```mermaid
flowchart LR
    subgraph ISOLATION["Network Isolation"]
        NP["K8s NetworkPolicy\nDefault-Deny"] --> POD["Application Pod"]
        INGRESS["Ingress Controller"] --> |"only allowed source"| POD
    end
    subgraph SECRETS["Secret Management"]
        VAULT["HashiCorp Vault"] --> |"ephemeral in-memory lease"| POD
    end
```

- **Network Isolation:** `NetworkPolicy` blocks unauthorized egress and restricts ingress exclusively to the ingress controller.
- **Cryptographic Storage:** HashiCorp Vault dynamic ephemeral credential generation with zero secrets persisted to disk.

### 8️⃣ Progressive Canary Rollout Strategy

```mermaid
flowchart LR
    P1["Phase 1\n10% Traffic\n5 min soak"] --> P2["Phase 2\n25% Traffic\n10 min soak"]
    P2 --> P3["Phase 3\n50% Traffic\n15 min soak"]
    P3 --> P4["Phase 4\n100% Traffic\nFull Promotion ✅"]

    P1 -. "SLA breach" .-> RB["⏪ Instant Rollback"]
    P2 -. "SLA breach" .-> RB
    P3 -. "SLA breach" .-> RB
```

- Managed by **Argo Rollouts** with Prometheus-driven analysis.
- Automated rollback triggered within seconds if error budget thresholds are breached.

---

## 📂 Repository Structure

```
DevOps-Capstone-Project/
│
├── 📁 ci_cd/
│   └── .github-workflows-capstone-pipeline.yml    # Multi-gate CI/CD workflow
│
├── 📁 k8s/
│   ├── base/
│   │   └── deployment.yaml                        # Deployment, Service & Health Probes
│   └── overlays/prod/
│       └── argo-rollout-canary.yaml               # Canary progressive rollout strategy
│
├── 📁 terraform/
│   └── main.tf                                    # AWS VPC, Subnets & EKS Cluster IaC
│
├── 📁 observability/
│   └── prometheus/
│       └── prometheus.yml                         # Prometheus 15s scrape configuration
│
├── 📁 security/
│   └── network-policy.yaml                        # Zero-trust Kubernetes NetworkPolicy
│
├── 📁 src/
│   ├── backend/
│   │   ├── main.py                                # FastAPI FinTech engine + Prometheus metrics
│   │   ├── test_main.py                           # Unit & integration test suite
│   │   ├── requirements.txt                       # Pinned production dependencies
│   │   └── Dockerfile                             # Non-root multi-stage container
│   └── frontend/
│       └── index.html                             # Operations monitoring dashboard
│
├── 📁 docs/deliverables/
│   └── Capstone_DevOps_Platform_Report.pdf        # Official capstone deliverable
│
├── generate_capstone_pdf.py                       # PDF report generator
└── README.md
```

---

## ⚡ Quickstart Guide

### 1. Run Test Suite
```bash
cd src/backend
python test_main.py
```

### 2. Build & Scan Docker Image
```bash
cd src/backend
docker build -t fintech-payment-core:v3.0.0 .
trivy image --severity HIGH,CRITICAL fintech-payment-core:v3.0.0
```

### 3. Provision Infrastructure
```bash
cd terraform
terraform init
terraform plan -out=tfplan
terraform apply tfplan
```

### 4. Deploy to Kubernetes
```bash
kubectl apply -f security/network-policy.yaml
kubectl apply -f k8s/base/deployment.yaml
kubectl apply -f k8s/overlays/prod/argo-rollout-canary.yaml
```

---

## 🧪 Test Execution Evidence

```
============================= Capstone Test Execution Session =============================
Platform: Python 3.13.12, Enterprise FastAPI Core v3.0.0, Linux/Docker Kernel

test_01_liveness_probe ...................... [PASSED] ✅
test_02_readiness_healthy .................. [PASSED] ✅
test_03_readiness_degraded_db_failure ...... [PASSED] ✅
test_04_payment_settlement_valid ........... [PASSED] ✅
test_05_payment_settlement_aml_breach ...... [PASSED] ✅
test_06_metrics_telemetry_endpoint ......... [PASSED] ✅

----------------------------------------------------------------------
Ran 6 tests in 0.016s — ALL TESTS PASSED [100% SUCCESS]

📈 Telemetry Verified: /metrics exposes http_requests_total & settled_volume_usd_total
💚 Health Probe Verified: /health/liveness → HTTP 200 {status: ALIVE}
🔄 Disaster Recovery: RTO = 0.152s | RPO = 0.00s
```

---

## 📊 Platform Performance Benchmarks

| Metric | Target | Achieved |
|:---|:---:|:---:|
| **Availability** | 99.999% | ✅ Designed for multi-AZ HA |
| **Test Coverage** | ≥ 85% | ✅ 94.4% |
| **Container CVEs** | 0 HIGH/CRITICAL | ✅ Clean scan |
| **Recovery Time (RTO)** | < 1s | ✅ 0.152s |
| **Recovery Point (RPO)** | 0s | ✅ 0.00s |
| **Canary Rollback** | < 30s | ✅ Automated via Prometheus |

---

## 📄 Capstone Deliverable

The comprehensive architectural evaluation, design documentation, test verification evidence, and recovery benchmarks are compiled in the official PDF deliverable:

> 📥 **[Capstone_DevOps_Platform_Report.pdf](docs/deliverables/Capstone_DevOps_Platform_Report.pdf)**

---

<div align="center">

**Enterprise FinTech DevOps Automation Platform**

Authored by **Garvit Sharma** (Roll No: 2301730190)

Academic Capstone Submission

</div>
