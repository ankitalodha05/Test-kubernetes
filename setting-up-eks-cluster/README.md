# Kubernetes Cluster Creation and Deployment Guide using ECR Image

## Prerequisites
1. **AWS CLI Installed**
2. **WSL (Windows Subsystem for Linux) Installed**
3. **eksctl Installed**
4. **kubectl Installed**
5. **Docker Image in ECR Repository**

---

## Step 1: AWS Configuration

### On Command Prompt (CMD):
```bash
aws configure
```

### On WSL:
```bash
aws configure
```

---

## Step 2: Install eksctl

Run the following commands in WSL:
```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
```

### Verify Installation
```bash
eksctl version
```

---

## Step 3: Install kubectl (if not already installed)
Follow the official documentation to install kubectl if required.

---

## Step 4: VPC and Subnet Configuration
1. Navigate to the AWS Management Console.
2. Go to the **VPC** section and select the required VPC.
3. Identify the **Subnet IDs** for the required region (e.g., Ohio).

---

## Step 5: Create EKS Cluster

Run the following command in CMD:
```bash
eksctl create cluster --region us-east-2 --name mdak-cluster --version 1.31 --nodegroup-name mdak-ng --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3 --vpc-public-subnets=subnet-01e04e7d828c4446c,subnet-0f2753059f35e2cde
```

---

## Step 6: Push Docker Image to ECR

1. Clone the repository containing your Dockerfile:
   ```bash
   git clone <repository-url>
   ```
2. Create an ECR repository on the AWS Console.
3. Follow the push commands provided in the ECR console to upload the Docker image:
   ```bash
   aws ecr get-login-password --region us-east-2 | docker login --username AWS --password-stdin <your-ecr-repo-url>
   docker build -t <image-name> .
   docker tag <image-name>:latest <your-ecr-repo-url>:latest
   docker push <your-ecr-repo-url>:latest
   ```

---

## Step 7: Create Kubernetes Secrets

Run the following command to create a secret for ECR:
```bash
kubectl create secret docker-registry ecr-secret --docker-server=746669215817.dkr.ecr.us-east-2.amazonaws.com --docker-username=AWS --docker-password=$(aws ecr get-login-password --region us-east-2) --docker-email=your-email@example.com
```

---

## Step 8: Create Deployment YAML

Create a file `deployment.yaml` with the following content:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: default
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      imagePullSecrets:
      - name: ecr-secret
      containers:
      - name: nginx
        image: <your-ecr-repo-url>:latest
        ports:
        - containerPort: 80
```

---

## Step 9: Create Load Balancer Service YAML

Create a file `svc-lb.yaml` with the following content:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
```

---

## Step 10: Apply YAML Files

Run the following commands:
```bash
kubectl apply -f deployment.yaml
kubectl apply -f svc-lb.yaml
```

### Verify Deployment:
```bash
kubectl get deployment
```

### Verify Service:
```bash
kubectl get svc
```

Copy the domain name from the output and check in a browser.

---

## Step 11: Delete Cluster

To delete the cluster, run:
```bash
eksctl delete cluster --name mdak-cluster --region us-east-2
```

