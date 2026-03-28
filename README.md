# Sample Django App on Kubernetes

This project is a **Django application** deployed on **Docker Desktop Kubernetes**.   It demonstrates how to build, deploy, and access a Django app using Docker and Kubernetes.

This Django project is packaged as a Docker image. Users can deploy it directly on Kubernetes without needing the source code.

---

## Prerequisites

- Mac / Linux / Windows
- [Docker Desktop](https://www.docker.com/products/docker-desktop) (with Kubernetes enabled)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- Python 3.x (for local development)
- Git

---

## Step 1 — Pull the Docker image

```bash
docker pull saim2026/k8s-sample-proj:v1
```

> This image contains the Django app, all dependencies, and templates.

---

## Step 2 — Deploy the image on Kubernetes


Create a deployment using the pulled image:

```bash
kubectl create deployment sample-python-app --image=saim2026/k8s-sample-proj:v1
```

Check that the pod is running:

```bash
kubectl get pods
```

---

## Step 3 — Expose the deployment via NodePort

```bash
kubectl expose deployment sample-python-app --type=NodePort --port=8000
kubectl get svc sample-python-app
```

Note the NodePort (e.g., 30480) to access the app.


---

## Step 4 — Access the app

### Option 1 — Port-forward (recommended for local Docker Desktop)

```bash
kubectl port-forward deployment/sample-python-app 8000:8000
```

Then open in your browser:

```bash
http://localhost:8000/demo/
```

Or curl:

```bash
curl http://localhost:8000/demo
```

### Option 2 — NodePort

Use the NodePort obtained in Step 3:

```bash
curl http://localhost:<NodePort>/demo
```

---

### Step 5 — Updating the app

If a new image version is released:

```bash
kubectl set image deployment/sample-python-app sample-python-app=saim2026/k8s-sample-proj<new-version>
```

<new-version> could be v2, v3, latest etc.

Kubernetes will automatically rollout the update to all pods.

---

### Step 6 — Clean up

To remove the deployment and service:

```bash
kubectl delete deployment sample-python-app
kubectl delete svc sample-python-app
```

---



# Sample Python Django App on Kubernetes (Minikube)

This Django project is packaged as a Docker image. Users can deploy it on Minikube without needing the source code.

## Prerequisites

- Minikube installed (https://minikube.sigs.k8s.io/docs/start/)
- kubectl installed
- Internet access to pull the Docker image


## Step 1 — Start Minikube
```bash
minikube start
```

Wait for Minikube to start and confirm nodes are ready:
```bash
kubectl get nodes
```
---

## Step 2 — Pull Docker image


Since Minikube has its own Docker environment, you can use your Docker Hub image:
```bash
kubectl create deployment sample-python-app --image=saim2026/k8s-sample-proj:v1
```

This creates a pod running your Django app inside Minikube.

Verify pod is running:
```bash
kubectl get pods
```


## Step 3 — Expose deployment
Expose via NodePort:
```bash
kubectl expose deployment sample-python-app --type=NodePort --port=8000
kubectl get svc sample-python-app
```

Note the NodePort (e.g., 30480) assigned to the service.


## Step 4 — Access the app

Option 1 — Using Minikube service

```bash
Minikube has a helper to open the service in the browser:
minikube service sample-python-app
```

This automatically opens the app in your default browser at the correct Minikube IP and NodePort.


## Option 2 — Access via NodePort manually

Get Minikube IP:

```bash
minikube ip
```

Then in browser or curl:

```bash
http://<minikube-ip>:<NodePort>/demo
curl http://<minikube-ip>:<NodePort>/demo
```


## Step 5 — Updating the app

If a new image version is released:

```bash
kubectl set image deployment/sample-python-app sample-python-app=saim2026/k8s-sample-proj:<new-version>
```

Kubernetes will automatically rollout the update.


## Step 6 — Clean up

To remove the deployment and service:

```bash
kubectl delete deployment sample-python-app
kubectl delete svc sample-python-app
```
Stop Minikube if desired:
```bash
minikube stop
```

### ✅ Notes for Minikube users

- The **image** is fully self-contained, so the user does not need any code download from any GitHub repo.
- The **/demo** endpoint is the main route for the Django app.
- **Minikube** automatically handles networking differently than Docker Desktop; use minikube service or <minikube-ip>:NodePort.


----
