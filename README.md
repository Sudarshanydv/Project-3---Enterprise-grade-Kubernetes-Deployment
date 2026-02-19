# Project 3 - ENTERPRISE-GRADE KUBERNETES DEPLOYMENT..........
 
## Complete Step-by-Step Guide  
 
> **What you will build (in simple words):** 
> You will deploy a **production-ready web application** on **Kubernetes (AWS EKS)** using **enterprise best practices** like namespaces, deployments, services, configmaps, secrets, autoscaling, and rolling updates.

This guide is written in **very simple, clear English**, exactly how **real DevOps engineers work in companies**.
   
---

## 1️⃣ What Does “Enterprise-Grade Kubernetes Deployment” Mean?

### Simple Explanation

Enterprise-grade means:

* Application is **highly available**
* Easy to **update without downtime**
* **Secure** configuration
* **Auto-scalable**
* **Monitored and manageable**

Companies never run just one container. They use **Kubernetes** to manage everything automatically.

---

## 2️⃣ Enterprise Architecture Flow

User → Load Balancer → Kubernetes Service → Pods → Containers → Application

---

## 3️⃣ Tools Used (Industry Standard)

| Tool       | Why used            |
| ---------- | ------------------- |
| Docker     | Package application |
| Docker Hub | Store images        |
| AWS EKS    | Managed Kubernetes  |
| kubectl    | Control cluster     |
| YAML       | Kubernetes configs  |
| HPA        | Auto scaling        |
| ConfigMap  | App config          |
| Secret     | Sensitive data      |

---

## 4️⃣ Step 1: Create Application Code

### Where to do this

📍 On your **local system** (VS Code / any editor)

### Example App (Python Flask)

**File name:** `app.py`

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return "Enterprise Kubernetes App is running successfully"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

👉 **Result:** This is your web app.

---

## 5️⃣ Step 2: Create Dockerfile (Containerization)

### Where to add

📍 Same folder as `app.py`

**File name:** `Dockerfile`

```dockerfile
FROM python:3.9-slim
WORKDIR /app
COPY app.py .
RUN pip install flask
EXPOSE 5000
CMD ["python", "app.py"]
```

---

## 6️⃣ Step 3: Build and Test Docker Image

### Commands (Terminal)

```bash
docker build -t enterprise-app:v1 .
docker run -d -p 5000:5000 enterprise-app:v1
```

### Test

Open browser:

```
http://localhost:5000
```

👉 **Result:** Message displayed in browser.

---

## 7️⃣ Step 4: Push Image to Docker Hub

```bash
docker tag enterprise-app:v1 yourdockerhub/enterprise-app:v1
docker push yourdockerhub/enterprise-app:v1
```

👉 **Result:** Image available for Kubernetes.

---

## 8️⃣ Step 5: Create EKS Cluster

```bash
eksctl create cluster --name enterprise-cluster --region ap-south-1
```

---

## 9️⃣ Step 6: Connect kubectl to EKS

```bash
aws eks update-kubeconfig --region ap-south-1 --name enterprise-cluster
kubectl get nodes
```

👉 **Result:** Worker nodes shown.

---

## 🔟 Step 7: Create Namespace (Enterprise Practice)

### Why?

Separates environments (dev, test, prod).

```bash
kubectl create namespace production
```

---

## 1️⃣1️⃣ Step 8: Create Deployment

### Where to add

📍 Create a file named `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: enterprise-deployment
  namespace: production
spec:
  replicas: 3
  selector:
    matchLabels:
      app: enterprise-app
  template:
    metadata:
      labels:
        app: enterprise-app
    spec:
      containers:
      - name: enterprise-container
        image: yourdockerhub/enterprise-app:v1
        ports:
        - containerPort: 5000
```

### Apply

```bash
kubectl apply -f deployment.yaml
kubectl get pods -n production
```

👉 **Result:** 3 running pods.

---

## 1️⃣2️⃣ Step 9: Create Service (LoadBalancer)

### File: `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: enterprise-service
  namespace: production
spec:
  type: LoadBalancer
  selector:
    app: enterprise-app
  ports:
  - port: 80
    targetPort: 5000
```

### Apply

```bash
kubectl apply -f service.yaml
kubectl get svc -n production
```

👉 **Result:** External IP created.

---

## 1️⃣3️⃣ Step 10: Access Application

Open browser:

```
http://<EXTERNAL-IP>
```

✔️ App live on internet.

---

## 1️⃣4️⃣ Step 11: ConfigMap (Enterprise Config)

### File: `configmap.yaml`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: production
data:
  APP_ENV: production
```

Apply:

```bash
kubectl apply -f configmap.yaml
```

---

## 1️⃣5️⃣ Step 12: Secret (Sensitive Data)

```bash
kubectl create secret generic app-secret \
--from-literal=DB_PASSWORD=admin123 \
-n production
```

---

## 1️⃣6️⃣ Step 13: Auto Scaling (HPA)

```bash
kubectl autoscale deployment enterprise-deployment \
--cpu-percent=50 --min=3 --max=10 -n production
```

Check:

```bash
kubectl get hpa -n production
```

---

## 1️⃣7️⃣ Step 14: Rolling Update (Zero Downtime)

```bash
kubectl set image deployment/enterprise-deployment \
enterprise-container=yourdockerhub/enterprise-app:v2 -n production
```

👉 **Result:** Pods update one-by-one.

---

## 1️⃣8️⃣ Final Output (What You Achieved)

✔️ Enterprise Kubernetes deployment
✔️ High availability
✔️ Load balanced traffic
✔️ Auto scaling enabled
✔️ Zero downtime updates

---

## 1️⃣9️⃣ Resume Project Description

> "Deployed an enterprise-grade Kubernetes application on AWS EKS using Docker, namespaces, deployments, services, ConfigMaps, Secrets, HPA, and rolling updates to achieve scalable, secure, and highly available infrastructure."

---

## 2️⃣0️⃣ Interview Explanation 

> "I containerized the application, deployed it on AWS EKS using Kubernetes best practices like namespaces, services, autoscaling, and rolling updates, ensuring high availability and zero downtime."

---
