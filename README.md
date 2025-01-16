# ArgoCD Applications

This repository contains multiple ArgoCD application configurations for managing Kubernetes resources using GitOps.

## Overview

Each application in this repository is defined by an ArgoCD `Application` manifest and associated Kubernetes manifests. The applications are structured to be deployed across various environments, utilizing ArgoCD for continuous deployment.

## Usage

- Clone this repository and apply the `applicatoin.yaml` files to your ArgoCD instance using `kubectl`:
  
  ```bash
  kubectl apply -f apps/app1/application.yaml

