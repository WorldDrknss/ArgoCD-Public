# ArgoCD Application - Traefik

This repository provides an ArgoCD configuration for deploying **Traefik**, a modern HTTP reverse proxy and load balancer for easily managing microservices, using Helm charts and custom configurations from a Git repository.

## Prerequisites

- **ArgoCD** installed and configured on your Kubernetes cluster.
- Access to the Kubernetes environment where Traefik will be deployed.
- Ensure required permissions for the Helm chart and Git repositories.

## Overview

This ArgoCD application manages the deployment of the following components:

1. **Traefik** from its official Helm chart repository (`https://traefik.github.io/charts`).
2. **Custom Traefik configurations** from an additional Git repository (`https://github.com/WorldDrknss/argocd-traefik.git`).

### Key Features:
- Deploys Traefik in the `traefik` namespace, which is automatically created if it doesn't exist.
- Utilizes **Traefik** version 33.2.1 as defined in the Helm chart.
- Incorporates custom settings from the `argocd-traefik` GitHub repository.

## Application Configuration

The ArgoCD application configuration (`traefik-argocd.yaml`) is structured as follows:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: traefik
  namespace: argocd
spec:
  project: default
  sources:
    - repoURL: https://traefik.github.io/charts
      targetRevision: 33.2.1
      chart: traefik
      helm:
        valueFiles:
        - $values/argocd-traefik/values.yaml
    - repoURL: https://github.com/WorldDrknss/ArgoCD-Public.git
      path: "argocd-traefik/manifests"
      targetRevision: main
      ref: values
  destination:
    server: https://kubernetes.default.svc
    namespace: traefik
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
