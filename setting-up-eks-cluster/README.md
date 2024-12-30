# Setting Up an EKS Cluster

## Step 1: Configure AWS CLI

### Open Command Prompt (CMD):
```bash
aws configure
```

### Open WSL (Windows Subsystem for Linux):
```bash
aws configure
```

## Step 2: Install `eksctl`

### Download and Install `eksctl`:
```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp && sudo mv /tmp/eksctl /usr/local/bin
```

### Verify the Installation:
```bash
eksctl version
```

## Step 3: Configure VPC and Subnets

### Ohio Region (us-east-2):
1. Locate the required **VPC** and **Subnet IDs** for your cluster.
2. Ensure you identify the public subnets in the correct region by scrolling to the right in your AWS Management Console to view details.

## Step 4: Create an EKS Cluster

### Run the following command to create the cluster in **Ohio Region (us-east-2):**
```bash
eksctl create cluster --region us-east-2 --name mdak-cluster --version 1.31 --nodegroup-name mdak-ng --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3 --vpc-public-subnets=subnet-01e04e7d828c4446c,subnet-0f2753059f35e2cde
```

