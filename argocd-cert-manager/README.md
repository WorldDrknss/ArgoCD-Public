# ArgoCD Application - Cert-Manager

This repository contains an ArgoCD application configuration for deploying **Cert-Manager**, a Kubernetes add-on to automate the management and issuance of TLS certificates from various issuing sources. Cert-Manager makes it easier to manage certificates, ensuring they are issued and renewed automatically.

## Prerequisites

- **ArgoCD** installed and configured on your Kubernetes cluster.
- Access to the Kubernetes environment where Cert-Manager will be deployed.
- Ensure that the necessary Helm chart repositories and Git repositories are accessible.

## Overview

The ArgoCD application is set up to deploy the following:

1. **Cert-Manager** from the official Jetstack Helm chart repository (`https://charts.jetstack.io`).
2. **Custom Cert-Manager configurations** from a Git repository (`https://github.com/WorldDrknss/ArgoCD-Public.git`).

### Key Features:
- Automatically creates the namespace `cert-manager` if it doesn't already exist.
- Utilizes **Cert-Manager** version v1.16.2 from the official Helm chart.
- Deploys custom configurations defined in the `argocd-cert-manager` repository.

## Application Configuration

The ArgoCD application configuration (`application.yaml`) is as follows:

```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: cert-manager
  namespace: argocd
spec:
  project: default
  sources:
    - repoURL: https://charts.jetstack.io
      chart: cert-manager
      targetRevision: v1.16.2
      helm:
        valueFiles:
          - $values/values.yaml
    - repoURL: https://github.com/WorldDrknss/argocd-cert-manager.git
      path: "."
      targetRevision: main
      ref: values
      directory:
        recurse: true
        exclude: '{values.yaml,LISCENSE,README.md}'
  destination:
    server: https://kubernetes.default.svc
    namespace: cert-manager
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
