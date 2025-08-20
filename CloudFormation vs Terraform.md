# CloudFormation vs Terraform

## How CloudFormation Works

When you run a **cluster create command** (for example, with `eksctl` or the AWS CLI), the request is passed to **AWS CloudFormation** in the backend.
CloudFormation then provisions all the required AWS resources (like VPCs, IAM roles, subnets, security groups, EKS control plane, node groups, etc.) to create the cluster.

👉 In short: **You give the command → CloudFormation templates are triggered → Cluster is created.**

---

## What is CloudFormation?

* **AWS CloudFormation** is an **Infrastructure as Code (IaC)** service provided natively by AWS.
* It lets you define resources in **JSON or YAML templates**, which AWS executes to provision infrastructure.
* It is tightly integrated with AWS and supports **only AWS resources**.

---

## CloudFormation vs Terraform

| Feature              | CloudFormation                      | Terraform                                |
| -------------------- | ----------------------------------- | ---------------------------------------- |
| **Provider**         | AWS native                          | Third-party, multi-cloud                 |
| **Language**         | JSON / YAML                         | HCL (HashiCorp Configuration Language)   |
| **Scope**            | Works only on AWS                   | Works on AWS, Azure, GCP, etc.           |
| **Integration**      | Deeply integrated with AWS services | Broad integrations across clouds & tools |
| **Learning Curve**   | Easier if only using AWS            | Slightly higher, but more powerful       |
| **State Management** | Managed internally by AWS           | Requires a backend (S3, GCS, etc.)       |

---

## Quick Summary

* **CloudFormation** → AWS native, works only in AWS.
* **Terraform** → Multi-cloud, open-source, works across providers.

**Interview Tip (10-sec answer):**

> “CloudFormation is AWS’s native IaC service, limited to AWS, while Terraform is a third-party, multi-cloud IaC tool. Both work declaratively, but Terraform is preferred for portability and ecosystem support.”

---
