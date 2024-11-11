# Beyond Kubernetes fundamentals

Before start check yout path.

```bash
cd files-beyond-kubernetes/
```

## Ingress

[minimal-ingress.yaml](./minimal-ingress.yaml)

### Esposing a web server using Ingress

```bash
# Create a new deployment
kubectl create deployment my-webserver --image=nginxdemos/hello

# Create a service with a command
# The service listens on :8080, but the container (our app) does on :80
kubectl expose deployment/my-webserver --name my-webserver --port=8080 --target-port=80


# Not required, but just to check it works, let’s port-forward it temporarily
# The service listened on :8080, but we will port-forward it through the :7777
kubectl port-forward service/my-webserver 7777:8080
```

Go to: http://127.0.0.1:7777/

[my-demo-ingress.yaml](./my-demo-ingress.yaml)

```bash
kubectl apply -f my-demo-ingress.yaml
kubectl get ingress
```

Go to: http://localhost:8080

## Persistence Volume Claim

[my-pvc.yaml](./my-pvc.yaml)

```bash
kubectl apply -f my-pvc.yaml
kubectl get pvc
```

[my-pod-pvc.yaml](./my-pod-pvc.yaml)

```bash
kubectl apply -f my-pod-pvc.yaml
```

## Job and CronJob

[my-job.yaml](./my-job.yaml)

```bash
kubectl apply -f my-job.yaml
kubectl get jobs
```

[my-cronjob.yaml](./my-cronjob.yaml)

```bash
kubectl apply -f my-cronjob.yaml
kubectl get cronjobs
kubectl get job
```

## InitContainers

[my-pod-init.yaml](./my-pod-init.yaml)

```bash
kubectl apply -f my-pod-init.yaml
kubectl get pods
```

## ConfigMap

[my-cm.yaml](./my-cm.yaml)

```bash
kubectl apply -f my-cm.yaml
```

Equivalent:

```bash
kubectl create configmap mysql --from-literal=replication-mode=master --from-literal=replication-user=master
```

check:

```bash
kubectl get configmap mysql -o jsonpath='{.data}'
```

[my-pod-cm-env.yaml](./my-pod-cm-env.yaml)

``` bash
kubectl apply -f my-pod-cm-env.yaml
```

[my-pod-cm-vol.yaml](./my-pod-cm-vol.yaml)

```bash
kubectl apply -f my-pod-cm-vol.yaml
```

## Secret

[my-secret.yaml](./my-secret.yaml)

```bash
kubectl apply -f my-secret.yaml
```

Equivalent:

```bash
kubectl create secret generic mysql --from-literal=password=root
```

Check:

```bash
kubectl get secret mysql -o go-template='{{.data.password | base64decode}}'
```

[my-pod-secret.yaml](./my-pod-secret.yaml)

```bash
kubectl apply -f my-pod-secret.yaml
```
