Here is the professional `README.md` file for **Mission 4: Configuration & Security (ConfigMaps & Secrets)**. 

In the real world, you never want to hardcode things like database passwords, API keys, or environment settings directly into your application code. In this mission, you decouple your configuration from your application using Kubernetes ConfigMaps (for non-sensitive data) and Secrets (for passwords).

You can create a new folder called `04-config-and-secrets` and paste this inside:

***

```markdown
# 🚀 Mission 4: Configuration & Security (ConfigMaps & Secrets)

## 🎯 Goal
The objective of this lab is to securely manage application configuration. We will decouple environment variables and sensitive credentials from the application code by injecting them into the Pods using ConfigMaps and Secrets.

## ⚙️ Environment & Prerequisites
* **Local Cluster:** Docker Desktop (with Kubernetes enabled via Kubeadm)
* **CLI Tool:** `kubectl`

## 📊 Configuration Workflow Diagram
This diagram illustrates how Kubernetes takes external configuration files and securely injects them into the running containers as environment variables.



## 📂 Files in this Mission
* `webapp-configmap.yaml`: Stores non-sensitive configuration data (like `APP_COLOR` or `LOG_LEVEL`).
* `webapp-secret.yaml`: Stores sensitive, base64-encoded credentials (like `DB_PASSWORD`).
* `webapp-deployment.yaml`: The deployment configured to read from the ConfigMap and Secret and expose them to the container.
* `screenshots/04-config-success.png`: Proof of successful environment variable injection.

---

## 🛠️ Step-by-Step Execution

### 1. Create the Configuration and Secret
First, we load the variables and credentials into the Kubernetes cluster:
```bash
kubectl apply -f webapp-configmap.yaml
kubectl apply -f webapp-secret.yaml
```

### 2. Deploy the Application
Next, we deploy the web app that is programmed to consume these configurations:
```bash
kubectl apply -f webapp-deployment.yaml
```

### 3. Verify the Objects
Ensure that the ConfigMap and Secret were created successfully:
```bash
kubectl get configmaps
kubectl get secrets
```

---

## 🧪 The "Secure Injection" Experiment
To prove that our configuration files worked, I accessed the inside of the running container and checked its internal environment variables to ensure the data was successfully injected without hardcoding it.

1.  **Find the running pod name:**
    ```bash
    kubectl get pods
    ```
2.  **Open a shell inside the pod:**
    ```bash
    kubectl exec -it <pod-name> -- /bin/sh
    ```
3.  **Print the environment variables:**
    ```bash
    echo "The App Color is: $APP_COLOR"
    echo "The Database Password is: $DB_PASSWORD"
    ```

### 🎉 Result
The terminal successfully printed out the exact variables defined in the ConfigMap and Secret (`$APP_COLOR` and `$DB_PASSWORD`). The application now has access to its configuration securely, without exposing credentials in the deployment blueprint!

---

## 💡 Key Learning Points
* **Decoupling Configuration:** I learned that separating configuration from code makes applications highly portable. I can use the exact same Docker image in Development, Staging, and Production simply by swapping out the ConfigMap.
* **Security via Secrets:** I learned how to store sensitive data securely using Kubernetes Secrets. While they are base64-encoded by default, they provide a standardized way to manage passwords and API keys without leaking them into version control (like GitHub).
* **Environment Variable Injection:** I demonstrated how to map specific keys from a ConfigMap or Secret directly into a container's environment space.
```