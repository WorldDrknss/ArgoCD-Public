# ArgoCD MetalLB Application

This repository contains an ArgoCD application configuration for deploying **MetalLB** using the Helm chart and custom configurations from a Git repository. MetalLB is a load-balancer implementation for Kubernetes clusters that enables the assignment of external IP addresses to services in a bare-metal environment.

## Prerequisites

- **ArgoCD** installed and configured on your Kubernetes cluster.
- Access to the Kubernetes cluster where MetalLB will be deployed.
- Ensure that the necessary Helm chart repositories and Git repositories are accessible.

## Overview

The ArgoCD application deploys the following:

1. **MetalLB** from the official MetalLB Helm repository (`https://metallb.github.io/metallb`).
2. **Custom MetalLB configurations** from a custom Git repository (`https://github.com/WorldDrknss/argocd-metallb.git`), which contains custom configurations for MetalLB's setup.

### Key Features:
- Automatically creates the namespace `metallb-system` for MetalLB if it doesn't already exist.
- Uses **MetalLB** version 0.14.9 from the official Helm chart.
- Deploys custom configurations from the `argocd-metallb` repository.

## Application Configuration

The ArgoCD application configuration (`metallb-argocd.yaml`) is as follows:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: metallb
  namespace: argocd
spec:
  project: default
  sources:
    - repoURL: https://metallb.github.io/metallb
      chart: metallb
      targetRevision: 0.14.9
      helm:
        valueFiles:
        - $values/argocd-metallb/values.yaml
    - repoURL: https://github.com/WorldDrknss/ArgoCD-Public.git
      path: "argocd-metallb/manifests"
      targetRevision: main
      ref: values
  destination:
    server: https://kubernetes.default.svc
    namespace: metallb-system
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
