# 🚀 Kubernetes Capstone Project: WordPress + MySQL Deployment

## 📌 Overview

This project demonstrates a **production-style Kubernetes deployment** of a WordPress application backed by MySQL, using core Kubernetes concepts such as:

* Namespaces
* Deployments & StatefulSets
* Services (ClusterIP, NodePort, Headless)
* ConfigMaps & Secrets
* Persistent Volumes (PVC)
* Resource Requests & Limits
* Probes (Liveness & Readiness)
* Horizontal Pod Autoscaler (HPA)
* Helm (comparison)

This capstone consolidates hands-on learning from multiple Kubernetes topics into a real-world application.

---

## 🏗️ Architecture

```
User → NodePort Service → WordPress Deployment → MySQL StatefulSet
                              ↓
                        ConfigMap + Secret
                              ↓
                         Persistent Storage
```

---

## 📂 Project Structure

```
.
├── namespace.yaml
├── mysql-secret.yaml
├── mysql-service.yaml
├── mysql-statefulset.yaml
├── wordpress-configmap.yaml
├── wordpress-deployment.yaml
├── wordpress-service.yaml
├── hpa.yaml
└── README.md
```

---

## ⚙️ Setup Instructions

### 1️⃣ Create Namespace

```bash
kubectl create namespace capstone
kubectl config set-context --current --namespace=capstone
```

---

### 2️⃣ Deploy MySQL (StatefulSet)

* Uses Secret for credentials
* Persistent storage for data

```bash
kubectl apply -f mysql-secret.yaml
kubectl apply -f mysql-service.yaml
kubectl apply -f mysql-statefulset.yaml
```

Verify:

```bash
kubectl get pods
kubectl exec -it mysql-0 -- mysql -u wordpress -p
```

---

### 3️⃣ Deploy WordPress

```bash
kubectl apply -f wordpress-configmap.yaml
kubectl apply -f wordpress-deployment.yaml
```

Verify:

```bash
kubectl get pods
```

---

### 4️⃣ Expose WordPress

```bash
kubectl apply -f wordpress-service.yaml
```

Access:

```bash
kubectl port-forward svc/wordpress 8080:80
```

---

## 🌐 Access Application

Open in browser:

```
http://localhost:8080
```

Complete WordPress setup and create a sample blog post.

---

## 🔁 Self-Healing Test

```bash
kubectl delete pod <wordpress-pod>
kubectl delete pod mysql-0
```

✔ Pods are automatically recreated
✔ Data remains intact

---

## 💾 Persistence Test

* Data stored via PVC
* Even after pod restart → data is preserved

---

## 📈 Horizontal Pod Autoscaler (HPA)

```bash
kubectl apply -f hpa.yaml
kubectl get hpa
```

* CPU target: 50%
* Min replicas: 2
* Max replicas: 10

---

## 🔐 Configuration Management

| Type      | Usage              |
| --------- | ------------------ |
| ConfigMap | DB host & DB name  |
| Secret    | DB user & password |

---

## ⚙️ Resource Management

```yaml
requests:
  cpu: 250m
  memory: 512Mi
limits:
  cpu: 500m
  memory: 1Gi
```

---

## ❤️ Health Checks

* **Liveness Probe** → restarts unhealthy container
* **Readiness Probe** → controls traffic

---

## 📊 Concepts Used

| Concept     | Description                |
| ----------- | -------------------------- |
| Namespace   | Resource isolation         |
| StatefulSet | MySQL with stable identity |
| Deployment  | WordPress scaling          |
| Service     | Networking                 |
| ConfigMap   | App configuration          |
| Secret      | Sensitive data             |
| PVC         | Persistent storage         |
| Probes      | Health checks              |
| HPA         | Auto scaling               |

---

## ⚖️ Helm Comparison (Bonus)

Helm simplifies deployment but reduces visibility/control compared to manual manifests.

    helm install wp-helm bitnami/wordpress
    kubectl get all  
    helm uninstall wp-helm 
---

## 🧹 Cleanup

```bash
kubectl delete namespace capstone
kubectl config set-context --current --namespace=default
```

---

## 📸 Output

✔ WordPress UI running
✔ Pods healthy
✔ HPA configured

---

## 🧠 Learnings

* Real-world application deployment on Kubernetes
* Managing stateful + stateless workloads together
* Importance of probes and resource limits
* Understanding Kubernetes networking & DNS


