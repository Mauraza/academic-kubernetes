# Helm Charts

## [Install helm](https://helm.sh/docs/intro/install/)

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Basic commands

```bash
# Add a new chart repository
helm repo add bitnami https://charts.bitnami.com/bitnami

# Explore the repository
helm search repo bitnami

# Install an helm chart
helm install my-release bitnami/apache

# Install an application
helm install my-release oci://registry-1.docker.io/bitnamicharts/apache

# List the installed releases in the cluster
helm list

```
