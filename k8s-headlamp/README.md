# Headlamp Helm Chart

Headlamp is an easy-to-use and extensible Kubernetes web UI that provides:
- 🚀 Modern, fast, and responsive interface
- 🔒 OIDC authentication support
- 🔌 Plugin system for extensibility
- 🎯 Real-time cluster state updates

## Prerequisites

- Kubernetes 1.21+
- Helm 3.x
- Cluster read-only access for initial setup

## Quick Start

### Step 1: Apply RBAC

This RBAC configuration applies read-only access to Headlamp. It creates a ClusterRole with read-only permissions, a ServiceAccount, and binds them together using a ClusterRoleBinding.

```bash
kubectl apply -f rbac/
```

### Step 2: Create Namespace (Optional)

```bash
kubectl create namespace observability
```

### Step 3: Deploy Headlamp via Helm

Add the Headlamp repository and install the chart:

```bash
# Add Headlamp Helm repository
helm repo add headlamp https://kubernetes-sigs.github.io/headlamp/
helm repo update

# Install Headlamp with your custom values.yaml
helm install headlamp headlamp/headlamp --namespace observability -f values.yaml

# Update Headlamp with your custom values.yaml
helm upgrade headlamp headlamp/headlamp --namespace observability -f values.yaml
```

## Access Headlamp

Use port-forwarding to access Headlamp:

```bash
kubectl port-forward -n observability svc/headlamp 8080:80
```

Then open http://localhost:8080 in your browser.

## Additional Installation Options

### Basic Installation

```bash
helm install headlamp headlamp/headlamp --namespace observability
```

### Installation with OIDC

```bash
helm install headlamp headlamp/headlamp \
  --namespace observability \
  --set config.oidc.clientID=your-client-id \
  --set config.oidc.clientSecret=your-client-secret \
  --set config.oidc.issuerURL=https://your-issuer-url
```

### Installation with Ingress

```bash
helm install headlamp headlamp/headlamp \
  --namespace observability \
  --set ingress.enabled=true \
  --set ingress.hosts[0].host=headlamp.example.com \
  --set ingress.hosts[0].paths[0].path=/
```