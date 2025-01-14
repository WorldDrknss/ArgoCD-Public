# ArgoCD Vault Application

This repository contains an ArgoCD application configuration for deploying **HashiCorp Vault** using the Helm chart and custom configurations from a Git repository. Vault is used for securely storing and accessing secrets, and this setup includes configuring Vault with Raft storage for highly available, easy-to-deploy secrets management.

## Prerequisites

- **ArgoCD** installed and configured on your Kubernetes cluster.
- AWS DynamoDB and AWS KMS Services
- Access to the Kubernetes cluster where Vault will be deployed.
- Ensure that the necessary Helm chart repositories and Git repositories are accessible.

## Overview

The ArgoCD application deploys the following:

1. **HashiCorp Vault** from the official Helm repository (`https://helm.releases.hashicorp.com`).
2. **Vault DynamoDB Storage** from a custom Git repository (`https://github.com/WorldDrknss/ArgoCD-Public.git`), which contains custom configurations for Vault's storage backend and other Vault settings.
3. **AWS KMS** for Vault auto unsealing.

### Key Features:
- Automatically creates the namespace `hashicorp` for Vault if it doesn't already exist.
- Uses `Vault` version 0.29.1 from the official Helm chart.
- Deploys custom configurations from the `argocd-vault-ddm-storage` repository.

## Application Configuration

The ArgoCD application configuration (`appplication.yaml`) is as follows:

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
        - $values/argocd-vault-ddm-storage/values.yaml
    - repoURL: https://github.com/WorldDrknss/ArgoCD-Public.git
      path: "argocd-vault-ddm-storage/manifests"
      targetRevision: main
      ref: values
  destination:
    namespace: hashicorp
    server: https://kubernetes.default.svc
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
