# ArgoCD Vault Application

This repository contains an ArgoCD application configuration for deploying **HashiCorp Vault** using the Helm chart and custom configurations from a Git repository. Vault is used for securely storing and accessing secrets, and this setup includes configuring Vault with Raft storage for highly available, easy-to-deploy secrets management.

## Prerequisites

- **ArgoCD** installed and configured on your Kubernetes cluster.
- Access to the Kubernetes cluster where Vault will be deployed.
- Ensure that the necessary Helm chart repositories and Git repositories are accessible.

## Overview

The ArgoCD application deploys the following:

1. **HashiCorp Vault** from the official Helm repository (`https://helm.releases.hashicorp.com`).
2. **Vault Raft Storage** from a custom Git repository (`https://github.com/WorldDrknss/argocd-vault-raft-storage.git`), which contains custom configurations for Vault's storage backend and other Vault settings.

### Key Features:
- Automatically creates the namespace `hashicorp` for Vault if it doesn't already exist.
- Uses `Vault` version 0.29.1 from the official Helm chart.
- Deploys custom configurations from the `argocd-vault-raft-storage` repository.

## Application Configuration

The ArgoCD application configuration (`vault-appplication.yaml`) is as follows:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: vault
  namespace: argocd
spec:
  project: default
  sources: 
    - repoURL: https://helm.releases.hashicorp.com
      chart: vault
      targetRevision: 0.29.1
      helm:
        valueFiles:
        - $values/values.yaml
    - repoURL: https://github.com/WorldDrknss/argocd-vault-raft-storage.git
      path: "."
      targetRevision: main
      ref: values
      directory:
        recurse: true
        exclude: '{values.yaml,LISCENSE,README.md}'
  destination:
    namespace: hashicorp
    server: https://kubernetes.default.svc
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
