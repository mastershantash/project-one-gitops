# Project One GitOps: Platform & Infrastructure Repository

This repository represents the **GitOps Single Source of Truth** for the Kubernetes cluster.

## Architecture

This decoupled repository isolates Kubernetes infrastructure and workload configurations from application source code:

* **Application Code Repo:** [`project-one`](https://github.com/mastershantash/project-one) (Python FastAPI, ML model, tests, Dockerfile).
* **GitOps Platform Repo:** `project-one-gitops` (Helm charts, ArgoCD Application CRDs, environment values).

```
project-one-gitops/
├── argocd/                         # ArgoCD Application manifests
│   ├── infra-app.yaml              # Manages ml-infra platform
│   ├── workload-app.yaml           # Manages ml-app serving workload
│   └── application.yaml            # Multi-app root manifest
└── helm/                           # Parameterized Helm charts
    ├── ml-infra/                   # Shared platform (Redis, Kafka, Kafka UI, Prometheus, Grafana, Secrets)
    └── ml-app/                     # Serving workload (Deployments, Services, HPA, Probes)
```

## Quick Start & Operations

### Apply to Cluster
```bash
microk8s kubectl apply -f argocd/application.yaml
```

### Inspect Applications
```bash
microk8s kubectl get applications -n argocd
```

### Trigger Hard Reconcile
```bash
microk8s kubectl annotate app ml-app ml-infra -n argocd argocd.argoproj.io/refresh=hard --overwrite
```
