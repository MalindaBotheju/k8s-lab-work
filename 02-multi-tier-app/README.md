Here is the complete, professional `README.md` file for Mission 2. It follows the exact same structure as your first mission so your GitHub portfolio looks consistent and organized.

You can create a new `README.md` file inside your `02-multi-tier-app` folder and paste this directly into it.

-----

````markdown
# 🚀 Mission 2: Multi-Tier Application (Web + Database)

## 🎯 Goal
The objective of this lab is to deploy a multi-tier application consisting of a web frontend and a backend database. This mission demonstrates internal cluster networking by securing the database from the public internet while exposing the frontend to users.

## ⚙️ Environment & Prerequisites
* **Local Cluster:** Docker Desktop (with Kubernetes enabled via Kubeadm)
* **CLI Tool:** `kubectl`

## 📊 Deployment Workflow Diagram
This diagram illustrates the separation of external and internal traffic. The user connects to the Web Frontend via a `NodePort`, and the Frontend securely communicates with the Redis Database via an internal `ClusterIP`.

![Multi-Tier Workflow Diagram](screenshots/02-multi-tier-workflow.png)

## 📂 Files in this Mission
* `redis-deployment.yaml`: Manages the Redis database pod.
* `redis-service.yaml`: A `ClusterIP` service that keeps the database strictly internal and inaccessible from the outside world.
* `web-deployment.yaml`: Manages the frontend web application pods.
* `web-service.yaml`: A `NodePort` service that exposes the frontend web application to the host machine.
* `screenshots/02-multi-tier-success.png`: Visual proof of the frontend successfully connecting to the backend.

---

## 🛠️ Step-by-Step Execution

### 1. Deploy the Backend (Database)
First, we establish the internal database and its secure network connection:
```bash
kubectl apply -f redis-deployment.yaml
kubectl apply -f redis-service.yaml
````

### 2\. Deploy the Frontend (Web App)

Next, we deploy the web application that will connect to the database:

```bash
kubectl apply -f web-deployment.yaml
kubectl apply -f web-service.yaml
```

### 3\. Verify the Pods and Services

Check that all pods are running and both services are configured with the correct types:

```bash
kubectl get pods
kubectl get services
```

*Expected Output: Web pods and Redis pod showing `Running`. The web service should be `NodePort` and the redis service should be `ClusterIP`.*

### 4\. Access the Web Application

Find the randomly assigned port number for the web frontend:

```bash
kubectl get service web-service
```

*Look under the `PORT(S)` column. Open a web browser and navigate to `http://localhost:<PORT-NUMBER>` to see the application interacting with the database.*

### 🎉 Result

Successfully accessed the web application, which securely read from/wrote to the internal Redis database\!

-----

## 💡 Key Learning Points

  * **Network Isolation (`ClusterIP` vs `NodePort`):** I learned how to protect sensitive components. By using `ClusterIP` for the database, it gets an internal IP address that only other pods in the cluster can reach. It is completely hidden from the outside network.
  * **Multi-Tier Architecture:** I successfully demonstrated how distinct components of an application (frontend and backend) can be decoupled into separate deployments but still communicate seamlessly using Kubernetes Services.
  * **Internal DNS:** I learned that Kubernetes has a built-in DNS system. The web application doesn't need to know the hardcoded IP address of the database; it just sends traffic to the name of the Redis service (`redis-service`), and Kubernetes routes it automatically.

<!-- end list -->

```
```