
---

````markdown
# Kubernetes: Pod, ReplicaSet, and Deployment

This document provides a consolidated explanation of **Pod**, **ReplicaSet**, and **Deployment** in Kubernetes, along with YAML examples, trainer notes, key comparisons, and common `kubectl` commands.

---

## 1. Pod

### Example: `pod.yaml`
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp
  labels:
    app: webapp
    type: frontend
spec:
  containers:
    - name: nginx-container
      image: nginx
````

### Explanation

* A **Pod** is the smallest deployable unit in Kubernetes.
* It can run one or more containers together.
* Pods created directly are **not self-healing**.

---

## 2. ReplicaSet

### Example: `replicaset.yaml`

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: web-rs
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
        type: frontend
    spec:
      containers:
        - name: nginx-container
          image: nginx
```

### Explanation

* Ensures the desired number of Pods (e.g., 3) are always running.
* Provides **self-healing** by recreating deleted Pods.
* Cannot handle updates automatically — you must delete & recreate to apply changes.

---

## 3. Deployment

### Example: `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
        type: frontend
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
```

### Explanation

* **Deployment** is the standard way to run workloads.
* Manages ReplicaSets which in turn manage Pods.
* Features:

  * Rolling updates
  * Rollbacks
  * Easy scaling
  * Version history

---

## 4. Comparison

| Resource       | Self-Healing | Scaling | Rolling Updates | Rollback |
| -------------- | ------------ | ------- | --------------- | -------- |
| **Pod**        | ❌ No         | ❌ No    | ❌ No            | ❌ No     |
| **ReplicaSet** | ✅ Yes        | ✅ Yes   | ❌ No            | ❌ No     |
| **Deployment** | ✅ Yes        | ✅ Yes   | ✅ Yes           | ✅ Yes    |

---

## 5. Relationship Flow

```text
Pod (one or more containers)
     ↑
ReplicaSet (ensures desired number of Pods)
     ↑
Deployment (manages ReplicaSets, adds updates/rollbacks)
```

---

## 6. Real-World Analogy

* **Pod = Worker** → does the job.
* **ReplicaSet = Supervisor** → ensures enough workers are present.
* **Deployment = Manager** → manages supervisors, updates, and rollbacks.

---

## 7. Common kubectl Commands

### Pods

```bash
kubectl apply -f pod.yaml                # Create Pod
kubectl get pods                         # List Pods
kubectl describe pod myapp               # Detailed Pod info
kubectl delete pod myapp                 # Delete a Pod
kubectl logs myapp                       # View container logs
kubectl exec -it myapp -- /bin/sh        # Access Pod shell
```

### ReplicaSets

```bash
kubectl apply -f replicaset.yaml         # Create ReplicaSet
kubectl get rs                           # List ReplicaSets
kubectl describe rs web-rs               # Describe ReplicaSet
kubectl delete rs web-rs                 # Delete ReplicaSet
kubectl scale rs web-rs --replicas=5     # Scale Pods via ReplicaSet
```

### Deployments

```bash
kubectl apply -f deployment.yaml         # Create Deployment
kubectl get deployments                  # List Deployments
kubectl describe deployment web-deployment   # Detailed info
kubectl delete deployment web-deployment     # Delete Deployment
kubectl scale deployment web-deployment --replicas=5   # Scale Pods
kubectl set image deployment/web-deployment nginx-container=nginx:1.27   # Rolling update
kubectl rollout status deployment/web-deployment        # Check rollout status
kubectl rollout undo deployment/web-deployment          # Rollback
```

---

## 8. Key Takeaways

* **Pods**: Smallest unit, no self-healing.
* **ReplicaSets**: Add self-healing, maintain desired state.
* **Deployments**: Add rolling updates, rollback, scaling — most used in real-world.

---

```

---

thanks
```
