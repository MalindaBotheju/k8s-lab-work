# 🚀 The Basics (Nginx Deployment)

## 🎯 Goal
The objective of this lab is to deploy a scalable web server using Nginx, expose it to the local network, and demonstrate Kubernetes' built-in self-healing capabilities.

## ⚙️ Environment & Prerequisites
* **Local Cluster:** Docker Desktop (with Kubernetes enabled via Kubeadm)
* **CLI Tool:** `kubectl`

## 📂 Files in this Mission
* `deployment.yaml`: Manages 2 replicas of the Nginx container. It ensures that the specified number of pods are always running.
* `service.yaml`: A `NodePort` service that acts as a load balancer, opening a port on the host machine to direct traffic into the pods.
* `screenshots/01-nginx-success.png`: Visual proof of the successful deployment.

---

## 🛠️ Step-by-Step Execution

### 1. Deploy the Application
First, apply the YAML configurations to build the deployment and the service:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
````

### 2\. Verify the Pods

Check that the Kubernetes manager successfully spun up the pods:

```bash
kubectl get pods
```

*Expected Output: Two pods with a status of `Running`.*

### 3\. Access the Web Server

Since the service is configured as a `NodePort`, find the randomly assigned port number:

```bash
kubectl get service nginx-service
```

*Look under the `PORT(S)` column (e.g., `80:31483/TCP`). Open a web browser and navigate to `http://localhost:31483`.*

### 🎉 Result

Successfully routed traffic from the local browser through the Kubernetes Service and into the Nginx Pods\!

-----

## 🧪 The "Chaos" Experiment: Testing Self-Healing

To prove that the Deployment object actively monitors the state of the application, I intentionally destroyed a running server to watch Kubernetes recover it.

1.  **Find a pod name:**
    ```bash
    kubectl get pods
    ```
2.  **Delete the pod manually:**
    ```bash
    kubectl delete pod <insert-pod-name-here>
    ```
3.  **Immediately check the pods again:**
    ```bash
    kubectl get pods
    ```

**Observation:** The moment the pod was deleted, Kubernetes recognized that the actual state (1 pod) did not match the desired state (`replicas: 2`). It instantly terminated the dead pod and spun up a brand new one to replace it, resulting in zero downtime for the deployment.

-----

## 💡 Key Learning Points

  * **Declarative Infrastructure:** You don't tell Kubernetes *how* to do things; you tell it *what* you want (e.g., "I want 2 Nginx pods"), and it handles the rest.
  * **Selectors and Labels:** I learned that the `selector` inside `service.yaml` must perfectly match the `labels` inside the `deployment.yaml` metadata. This is how the Service knows exactly which pods to send internet traffic to.