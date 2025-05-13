# Homelab Argo App Config

This repository contains the configuration files for deploying and managing applications in your homelab environment using ArgoCD. It includes application manifests and infrastructure bootstrap configurations.

## Repository Structure

```
apps/
    meditrack/
        aggregator-service.yaml
        appointment-scheduling-service.yaml
        doctor-record-service.yaml
        notification-service.yaml
        patient-record-service.yaml
infra/
    bootstrap/
        actions-runner-controller.yaml
        kyverno-policies.yaml
        kyverno.yaml
        reloader.yaml
        sealed-secrets.yaml
```

### `apps/`
This directory contains ArgoCD application manifests for deploying various services. Each YAML file corresponds to a specific service in the `meditrack` application suite:

- **`aggregator-service.yaml`**: Configuration for the Aggregator Service.
- **`appointment-scheduling-service.yaml`**: Configuration for the Appointment Scheduling Service.
- **`doctor-record-service.yaml`**: Configuration for the Doctor Record Service.
- **`notification-service.yaml`**: Configuration for the Notification Service.
- **`patient-record-service.yaml`**: Configuration for the Patient Record Service.

### `infra/bootstrap/`
This directory contains bootstrap configurations for essential infrastructure components:

- **`actions-runner-controller.yaml`**: Configuration for GitHub Actions Runner Controller.
- **`kyverno-policies.yaml`**: Policies for Kyverno.
- **`kyverno.yaml`**: Configuration for Kyverno.
- **`reloader.yaml`**: Configuration for Reloader.
- **`sealed-secrets.yaml`**: Configuration for Sealed Secrets.

## Usage

### Prerequisites
- [ArgoCD](https://argo-cd.readthedocs.io/) installed and configured.
- A Kubernetes cluster accessible from your environment.

### Deploying Applications
The `apps/` directory is monitored by a parent ArgoCD application. Any changes to the manifests in this directory will automatically trigger updates in the cluster. To manually apply changes, you can still use:

```bash
kubectl apply -f apps/meditrack/<service-name>.yaml
```

Replace `<service-name>` with the desired service manifest file.

### Bootstrapping Infrastructure
The `infra/` directory is also monitored by a parent ArgoCD application. Changes to the manifests in this directory will automatically trigger updates in the cluster. To manually apply changes, you can use:

```bash
kubectl apply -f infra/bootstrap/<component>.yaml
```

Replace `<component>` with the desired infrastructure component file.

## Contributing

Contributions are welcome! If you have suggestions for improvements or new features, feel free to open an issue or submit a pull request.

## License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

Happy deploying!