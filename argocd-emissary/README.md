# Emissary Ingress with ArgoCD

This repository provides a setup for using Emissary Ingress with ArgoCD to manage your Kubernetes applications.

## Prerequisites

- Kubernetes cluster
- ArgoCD installed and configured
- Emissary Ingress installed

## Installation

1. **Clone the repository:**

    ```sh
    git clone https://github.com/WorldDrknss/ArgoCD-Public.git
    cd argocd-emissary
    ```

2. **Apply the Emissary Ingress manifests:**

    ```sh
    kubectl apply -f manifests/application.yaml
    ```

## Configuration

### Emissary Ingress

Ensure that Emissary Ingress is properly configured to route traffic to your services. You can find example configurations in the `emissary-ingress/` directory.

### ArgoCD

ArgoCD applications are defined in the `argocd-apps/` directory. Customize these manifests to match your application's requirements.

## Usage

1. Access the ArgoCD UI to manage your applications:

    ```sh
    kubectl port-forward svc/argocd-server -n argocd 8080:443
    ```

    Open your browser and navigate to `https://localhost:8080`.

2. Use Emissary Ingress to expose your services. Refer to the example configurations for guidance.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
