# ArgoCD Linkerd

This repository contains `application.yaml` files to deploy Linkerd components using ArgoCD. The components include:

- Linkerd CRDs
- Linkerd Control Plane
- Linkerd Viz
- Linkerd Jaeger

## Prerequisites

- ArgoCD installed and configured
- Kubernetes cluster

## Deploying Linkerd Components

### 1. Linkerd CRDs

Apply the `application.yaml` for Linkerd CRDs:

```bash
kubectl apply -f argocd-linkerd/crds/application.yaml
```

### 2. Linkerd Control Plane

Apply the `application.yaml` for Linkerd Control Plane:

```bash
kubectl apply -f argocd-linkerd/control-plane/application.yaml
```

### 3. Linkerd Viz

Apply the `application.yaml` for Linkerd Viz:

```bash
kubectl apply -f argocd-linkerd/linkerd-viz/application.yaml
```

### 4. Linkerd Jaeger

Apply the `application.yaml` for Linkerd Jaeger:

```bash
kubectl apply -f argocd-linkerd/linkerd-jaeger/application.yaml
```

## License

This project is licensed under the MIT License.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.