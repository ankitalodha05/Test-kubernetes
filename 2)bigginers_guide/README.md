# Kubernetes for Beginners: Understanding How It Works

Kubernetes, an open-source container orchestration platform, simplifies the deployment, scaling, and management of containerized applications. This document introduces core Kubernetes concepts and workflows, guiding beginners through practical examples.

---

## Setting Up Kubernetes with Minikube

Minikube is a lightweight Kubernetes implementation that runs locally, useful for learning and testing Kubernetes. Below is a walkthrough of key commands and concepts:

### 1. Checking Minikube Status
Run the following command to check the status of your Minikube cluster:
```bash
minikube status
```
This command provides information about the cluster’s components, such as the control plane and kubelet.

### 2. Listing System Pods
To see the pods running in the Kubernetes system namespace:
```bash
kubectl get pods -n kube-system
```
System pods handle essential cluster functionality.

---

## Deploying Applications with Kubernetes

### 1. Creating a Deployment

1. Open or create a YAML configuration file for the deployment:
    ```bash
    nano deployment.yaml
    ```
    Example content for `deployment.yaml`:
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: nginx-deployment
    spec:
      selector:
        matchLabels:
          app: nginx
      replicas: 2  # tell deployment to run 2 pods matching this template
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: nginx:latest
            ports:
            - containerPort: 80
    ```
2. Apply the deployment:
    ```bash
    kubectl apply -f deployment.yaml
    ```

    **How It Works:**
    - The `kubectl apply` command submits the configuration to the Kubernetes API server.
    - The API server validates the configuration and stores it in `etcd`, the cluster’s distributed key-value store.

### 2. Verifying Pod Status
- List all pods:
    ```bash
    kubectl get pods
    ```
- Watch for real-time updates:
    ```bash
    kubectl get pods -w
    ```
- View additional pod details:
    ```bash
    kubectl get pods -o wide
    ```

---

## Exploring Pods

### 1. Inspecting Pod Details in `etcd`
To check the pod’s status stored in `etcd`:
```bash
kubectl get pod mypod -o json | jq .status
```
**Note:** Replace `mypod` with the actual pod name.

### 2. Checking Node Assignment
To determine the node where a pod is running:
```bash
kubectl get pod mypod -o json | jq .spec.nodeName
```
This reveals the scheduler’s decision on pod placement.

### 3. Inspecting Pod IP Address
To find the IP address assigned to a pod:
```bash
kubectl describe pod mypod | grep IP
```

---

## Interacting with Nodes and Containers

### 1. Accessing the Minikube Node
To SSH into the Minikube node:
```bash
minikube ssh
```

### 2. Listing Running Containers
Inside the Minikube node, view running containers:
```bash
docker ps
```

---

## Understanding the Workflow

1. **Pod Creation:** The YAML file defines the desired state of the deployment, including the number of replicas and the container image.
2. **API Server Interaction:** When you apply the configuration, the API server validates and stores it in `etcd`.
3. **Scheduling:** The scheduler assigns the pod to a specific node.
4. **Pod Initialization:** The container runtime pulls the image and starts the container, assigning an IP address.
5. **Observation:** You can monitor the pod’s status, node assignment, and IP address through various `kubectl` commands.

---

## Key Takeaways

- **Minikube:** Simplifies running Kubernetes locally for learning and development.
- **Kubernetes API Server:** Central control point that validates and stores configurations.
- **etcd:** Stores all cluster state data, including pod definitions and statuses.
- **Scheduler:** Decides where to place pods.
- **kubectl Commands:** Enable interaction with the cluster, from deployment to troubleshooting.

This workflow and command guide provide a foundational understanding for working with Kubernetes. With practice, you can expand into advanced topics like scaling, services, and persistent storage.
