# Homelab Argo App Config

This repository contains the configuration files for deploying and managing applications in your homelab environment using ArgoCD. It includes application manifests for both traditional Ingress and modern Gateway API implementations, along with infrastructure bootstrap configurations.

## Repository Structure

```
apps/
    meditrack/
        aggregator-service.yaml
        appointment-scheduling-service.yaml
        doctor-record-service.yaml
        doctor-record-service-gateway.yaml    # Gateway API implementation
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

## Application Deployment Strategies

### Traditional Ingress Implementation
Most services use traditional Kubernetes Ingress with Kong ingress controller:
- Standard blue-green deployments with Argo Rollouts
- TLS termination via cert-manager with Let's Encrypt
- Kong-based routing and load balancing

### Gateway API Implementation
The `doctor-record-service-gateway.yaml` showcases the modern Gateway API approach:
- **Gateway API v1.0.0** with Kong gateway controller
- **Blue-Green Deployments** with preview routing capabilities
- **Dual-domain Setup**:
  - Production: `https://dev.doctor-records.apps.meditrack-app.me`
  - Preview: `https://preview.dev.doctor-records.apps.meditrack-app.me`
- **Automatic TLS** with cert-manager and Let's Encrypt
- **Advanced Traffic Management** via HTTPRoute resources

### `apps/meditrack/`
This directory contains ArgoCD application manifests for deploying various microservices:

- **`aggregator-service.yaml`**: API aggregation service with traditional ingress
- **`appointment-scheduling-service.yaml`**: Appointment management service
- **`doctor-record-service.yaml`**: Doctor records with traditional ingress
- **`doctor-record-service-gateway.yaml`**: Doctor records with Gateway API (showcase implementation)
- **`notification-service.yaml`**: Notification delivery service
- **`patient-record-service.yaml`**: Patient records management service

### `infra/bootstrap/`
Infrastructure components automatically deployed via ArgoCD:

- **`actions-runner-controller.yaml`**: GitHub Actions self-hosted runners
- **`kyverno-policies.yaml`**: Kubernetes security and governance policies
- **`kyverno.yaml`**: Policy engine for Kubernetes
- **`reloader.yaml`**: Automatic pod restarts on ConfigMap/Secret changes
- **`sealed-secrets.yaml`**: Encrypted secrets management

## Architecture Features

### Traffic Management
- **Kong Ingress Controller**: Handles both traditional Ingress and Gateway API
- **Gateway API Support**: Modern Kubernetes networking with advanced traffic splitting
- **Blue-Green Deployments**: Zero-downtime deployments with preview environments
- **TLS Everywhere**: Automatic HTTPS certificates via cert-manager

### Security & Compliance
- **Kyverno Policies**: Enforce security standards and best practices
- **Sealed Secrets**: Encrypted secret management in Git
- **Network Policies**: Micro-segmentation at the pod level

### Operations
- **GitOps Workflow**: All changes via Git with ArgoCD automation
- **Automatic Reloading**: Pods restart automatically on configuration changes
- **Self-Hosted CI/CD**: GitHub Actions runners in Kubernetes

## Usage

### Prerequisites
- [ArgoCD](https://argo-cd.readthedocs.io/) installed and configured
- Kong ingress controller with Gateway API support
- cert-manager for TLS certificate management
- Kubernetes cluster with Gateway API CRDs

### Gateway API vs Traditional Ingress

**Traditional Ingress (Most Services)**:
```yaml
spec:
  ingress:
    enabled: true
    ingressClassName: kong
    annotations:
      cert-manager.io/cluster-issuer: letsencrypt-prod
```

**Gateway API (Modern Approach)**:
```yaml
spec:
  gateway:
    enabled: true
    gatewayClassName: kong
  httpRoute:
    enabled: true
    preview:
      enabled: true  # Enables preview routing
```

### Deploying Applications
Applications are automatically deployed via ArgoCD's App of Apps pattern. Manual deployment:

```bash
# Traditional ingress-based service
kubectl apply -f apps/meditrack/doctor-record-service.yaml

# Gateway API-based service
kubectl apply -f apps/meditrack/doctor-record-service-gateway.yaml
```

### Infrastructure Bootstrap
Infrastructure components are automatically managed by ArgoCD:

```bash
kubectl apply -f infra/bootstrap/
```

## Migration Path

### From Ingress to Gateway API
1. **Phase 1**: Deploy Gateway API version alongside existing Ingress
2. **Phase 2**: Test preview routing and traffic management
3. **Phase 3**: Switch DNS to Gateway API endpoints
4. **Phase 4**: Deprecate traditional Ingress resources

### Benefits of Gateway API
- **Enhanced Traffic Control**: Advanced routing, retries, timeouts
- **Preview Environments**: Test deployments before promotion
- **Better Resource Modeling**: Clear separation of concerns
- **Vendor Neutrality**: Standardized across ingress controllers

## Contributing

Contributions are welcome! When adding new services:
1. Consider Gateway API for new applications
2. Follow the established naming conventions
3. Include proper TLS configuration
4. Document any special routing requirements

## License

This repository is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

**Gateway API Ready • GitOps Powered • Production Tested**