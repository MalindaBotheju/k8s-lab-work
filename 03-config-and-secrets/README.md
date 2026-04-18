Since we just built a Database in Mission 2, there is a massive hidden problem we need to fix in **Mission 3**. 

Right now, if your Redis database Pod crashes or gets deleted, **all of your data is permanently erased.** Kubernetes Pods are "ephemeral" (temporary) by default. 

So, for Mission 3, we are going to learn **Persistent Storage (Volumes)**. We are going to attach a permanent hard drive to your database so that even if the Pod is destroyed, the data survives!

Here is the professional `README.md` for Mission 3. You can create a new folder called `03-persistent-storage` and paste this inside:

***

```markdown
# 🚀 Mission 3: Persistent Storage (Stateful Applications)

## 🎯 Goal
The objective of this lab is to solve the problem of ephemeral container storage by attaching a Persistent Volume (PV) to a database. This ensures that application data survives even if the database Pod crashes or is intentionally deleted.

## ⚙️ Environment & Prerequisites
* **Local Cluster:** Docker Desktop (with Kubernetes enabled via Kubeadm)
* **CLI Tool:** `kubectl`

## 📊 Deployment Workflow Diagram
This diagram shows how a Pod connects to a Persistent Volume Claim (PVC), which in turn requests actual physical storage from the Kubernetes cluster.

![Persistent Storage Diagram](screenshots/03-storage-workflow.png)

## 📂 Files in this Mission
* `redis-pvc.yaml`: A Persistent Volume Claim that requests a specific amount of permanent storage from the cluster.
* `redis-deployment.yaml`: The database deployment, updated to mount the PVC as a volume inside the container.
* `redis-service.yaml`: The internal `ClusterIP` network for the database.
* `screenshots/03-storage-success.png`: Proof that data survived a pod deletion.

---

## 🛠️ Step-by-Step Execution

### 1. Create the Storage Claim
Before deploying the database, we must request the storage space:
```bash
kubectl apply -f redis-pvc.yaml
```

### 2. Deploy the Database
Apply the deployment and service (which are configured to use the PVC we just created):
```bash
kubectl apply -f redis-deployment.yaml
kubectl apply -f redis-service.yaml
```

### 3. Verify the Storage
Check that the Persistent Volume Claim is officially "Bound" (attached successfully):
```bash
kubectl get pvc
```
*Expected Output: The status should say `Bound`.*

---

## 🧪 The "Data Survival" Experiment
To prove the storage is truly persistent, I injected data into the database, destroyed the database server, and verified the data was still there when a new server spun up.

1.  **Open a shell inside the Redis pod:**
    ```bash
    kubectl exec -it <redis-pod-name> -- redis-cli
    ```
2.  **Save some test data:**
    ```text
    set mykey "Kubernetes Storage is Awesome!"
    exit
    ```
3.  **Delete the Redis Pod (simulating a crash):**
    ```bash
    kubectl delete pod <redis-pod-name>
    ```
4.  **Wait for Kubernetes to recreate the Pod, then open a shell in the *new* Pod:**
    ```bash
    kubectl exec -it <new-redis-pod-name> -- redis-cli
    ```
5.  **Retrieve the data:**
    ```text
    get mykey
    ```

### 🎉 Result
The command returned `"Kubernetes Storage is Awesome!"`. Even though the original Pod was completely destroyed, the new Pod attached to the exact same Persistent Volume and loaded the saved data seamlessly!

---

## 💡 Key Learning Points
* **Ephemeral vs. Persistent:** By default, anything written inside a container disappears when the container dies. 
* **Decoupling Storage from Compute:** I learned that Pods (Compute) and Volumes (Storage) have separate lifecycles. I can safely destroy and upgrade my application servers without touching the underlying user data.
* **PVCs (Persistent Volume Claims):** I learned how to use a PVC as a "ticket" to request storage from the cluster automatically.
```

***

**Are you ready to create the `redis-pvc.yaml` file so we can actually build this permanent hard drive?**