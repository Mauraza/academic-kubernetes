# Introduction to kubernetes

Before start check your path.

```bash
cd files-intro-kubernetes/
```

## Hands-on

[mongo-pod.yaml](./mongo-pod.yaml)

Create pod:

```bash
kubectl create -f mongo-pod.yaml
kubectl get pod mongo
```

### Managing labels

```bash
# Create a new label on-the-fly
kubectl label pods mongo my-label=my-value
# Show labels in the output
kubectl get pods --show-labels
# Find pods having label "my-label" equals to "my-value"
kubectl get pods -l my-label=foo
# List pods with a new column showing the label value "my-label"
kubectl get pods -L my-label
```

[nginx-deploy.yaml](./nginx-deploy.yaml)

Create the deployment:

```bash
kubectl create -f nginx-deploy.yaml
kubectl get deployments
```

Modifying the deployment replicas:

```bash
# Scale up our deployment
kubectl scale deployment nginx --replicas=3
# Show all the deployments again
kubectl get deployments
```

Changing the image used in our deployment

```bash
# Replace the image used in the container "nginx"
kubectl set image deployment nginx nginx=nginx:1.26-bookworm --all
# Describe the deployment
kubectl describe deployment nginx
# Get all replicasets
kubectl get replicasets
```

Rolling back a deployment:

```bash
# Create a deployment without writing any YAML file :)
 kubectl create deployment bad-nginx --image=nginx

# But.. everything is a YAML
kubectl get deployment/bad-nginx -o yaml

# Replace the image with a non-existent one and record the changes in log
kubectl set image deployment bad-nginx nginx=nginx:bad --all --record

# The pod will be in "ErrImagePull" since the image does not exist
kubectl get pods -l app=bad-nginx

# Get the deployment rollout history
# In revision 1 we created the deployment
kubectl rollout history deployment/bad-nginx

# Let’s undo and come back to the previous revision
kubectl rollout undo deployment/bad-nginx

# Get the pods again, now it is working again
kubectl get pods
```

Creating a service: ClusterIP + port-forwarding:

```bash
# Create a new deployment (or use an existent one)
kubectl create deployment nginx-exposed --image=nginxdemos/hello

# Create a service with a command
# The service listens on :8080, but the container (our app) does on :80
kubectl expose deployment/nginx-exposed --name nginx-clusterip --port=8080 --target-port=80 --type=ClusterIP

# Local port forwarding (the service is still internal, though)
# The service listened on :8080, but we will port-forward it through the :7777
kubectl port-forward service/nginx-clusterip 7777:8080
```

Go to: <http://localhost:7777/>

[nginx-svc.yaml](./nginx-svc.yaml)

```bash
kubectl apply -f nginx-svc.yaml
kubectl get services
```

Accessing a ClusterIP service from inside:

```bash
# Get the service "nginx-clusterip", note that it listens on :8080
kubectl get service nginx-clusterip

# Run wget inside a Pod that will get deleted after running the command
kubectl run -it --rm --restart=Never busybox --image=busybox wget http://nginx-clusterip:8080
```
