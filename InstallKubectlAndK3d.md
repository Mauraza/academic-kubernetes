# Install tools

## Install [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

## Install [k3d](https://k3d.io/v5.7.4/#installation)

```bash
curl -s "https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh"| bash
```

Create a cluster:

```bash
k3d cluster create
```

Delete a cluster:

```bash
k3d cluster delete
```

Create a cluster with NodePort or Ingress:

```bash
# Exposing NodePort 30000 in the host system, port 30000
k3d cluster create -p "30000:30000@agent:0" --agents 1

# Exposing Ingress controller in the host system, port 8080
k3d cluster create -p "8080:80@loadbalancer" --agents 1
```

check kubernetes cluster is up and running:

```bash
kubectl cluster-info
kubectl get nodes
```
