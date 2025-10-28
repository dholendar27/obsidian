---
date created: 2025-10-28 10:20
---

## AWS Compute Services

In **Amazon Web Services (AWS)**, **Compute Services** provide the processing power required to run applications, workloads, and services in the cloud. These services handle everything from simple web apps to large-scale distributed systems and machine learning workloads.

---

### 1. [[Amazon EC2 (Elastic Compute Cloud)]]

**What it is:** Virtual servers in the cloud.\
**Use case:** Hosting web servers, databases, or any custom application.\
**Key features:**

- Choose instance types optimized for compute, memory, or storage.
- Scale manually or automatically (Auto Scaling).
- Pay-as-you-go pricing.

---

### 2. [[AWS Lambda]]

**What it is:** Serverless compute — run code without managing servers.\
**Use case:** Event-driven applications (e.g., responding to S3 uploads, API Gateway events).\
**Key features:**

- Supports multiple languages (Python, Node.js, Java, etc.).
- Automatically scales with requests.
- Pay only for execution time (per millisecond).

---

### 3. Amazon ECS (Elastic Container Service)

**What it is:** A container orchestration service for running Docker containers.\
**Use case:** Microservices and containerized applications.\
**Key features:**

- Integrates with EC2 or AWS Fargate.
- Deep AWS integration (CloudWatch, IAM, etc.).

---

### 4. Amazon EKS (Elastic Kubernetes Service)

**What it is:** Managed Kubernetes service.\
**Use case:** Running Kubernetes workloads without managing control planes.\
**Key features:**

- Fully compatible with upstream Kubernetes.
- Works with EC2 or Fargate as worker nodes.

---

### 5. AWS Fargate

**What it is:** Serverless compute engine for containers.\
**Use case:** Running containers without provisioning or managing servers.\
**Key features:**

- Works with ECS and EKS.
- You define container specs; AWS handles infrastructure.

---

### 6. AWS Batch

**What it is:** Managed batch computing service.\
**Use case:** Large-scale, parallel, or batch data processing workloads.\
**Key features:**

- Automatically provisions compute resources.
- Works with EC2, Spot, or Fargate.

---

### 7. AWS Elastic Beanstalk

**What it is:** Platform as a Service (PaaS) for deploying and scaling web apps.\
**Use case:** Developers who want to deploy apps quickly without managing infrastructure.\
**Key features:**

- Supports popular languages (Java, .NET, Python, etc.).
- Auto scaling, load balancing, and monitoring built-in.

---

### 8. AWS Outposts

**What it is:** Extends AWS infrastructure to on-premises environments.\
**Use case:** Hybrid cloud solutions.\
**Key features:**

- Run AWS services locally.
- Seamless integration with AWS cloud.

---

### 9. AWS Lightsail

**What it is:** Simplified cloud platform for small businesses and developers.\
**Use case:** Hosting simple web apps, blogs, or dev/test environments.\
**Key features:**

- Easy setup (virtual servers, databases, networking).
- Predictable monthly pricing.

---

### 10. AWS Compute Optimizer

**What it is:** Service that recommends the best compute resources.\
**Use case:** Cost optimization and performance tuning.\
**Key features:**

- Uses machine learning to analyze usage.
- Suggests right-sizing for EC2, Auto Scaling groups, etc.
