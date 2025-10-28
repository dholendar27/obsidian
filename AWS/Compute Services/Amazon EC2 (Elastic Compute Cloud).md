---
date created: 2025-10-28 10:56
---

The EC2 is also known as Elastic Compute Cloud, is just simply a virtual computer a set up remotely else. We can think it as an system that is virtually set up and running that we can access remotely.

**Amazon EC2 (Elastic Compute Cloud)** instances are **virtual machines** that run on physical servers managed by AWS.
When you **create an EC2 instance**, AWS uses a **hypervisor** (such as **Nitro Hypervisor**) to allocate and manage the underlying physical resources — including **CPU, memory, storage, and networking** — from the host server.
The hypervisor creates an **isolated virtual environment** (your EC2 instance) that behaves just like a physical server, allowing you to install an operating system and run applications as if it were a dedicated machine.

## Getting Started with EC2 Instances

EC2 instances are very customizable and can be perfectly configured to meet your exact requirements

### Launching an EC2 instance

#### 1. Sign in to AWS Management Console

- Go to [https://console.aws.amazon.com](https://console.aws.amazon.com/).
- Log in with your AWS account credentials.

![[aws console.png]]

---

### 2. Navigate to EC2

- In the search bar, type **EC2** and select **EC2** from the services list.
- This opens the EC2 Dashboard.

![[EC2 Search.png]]

---

### 3. Launch Instance

- Click **Launch Instance**.
- You’ll be taken to the **Launch an Instance** wizard.

![[EC2 Create Instance.png]]

---

### 4. Name and Tags

- Enter a **name** for your instance (for example, “MyEC2Server”).
- Optionally, add **tags** (key-value pairs) to help organize resources.

![[EC2 name and tag.png]]

---

### 5. Choose an Amazon Machine Image (AMI)

- Select an **AMI** (Amazon Machine Image) — this is the operating system for your instance.\
  Examples:

  - Amazon Linux 2023
  - Ubuntu Server 24.04 LTS
  - Windows Server

![[EC2 AMI.png]]

---

### **6. Choose an Instance Type**

- Select an instance type based on CPU, memory, and performance needs.\
  Example: **t2.micro** (eligible for free tier).

![[EC2 Instance type.png]]

---

### **7. Configure Key Pair (SSH Access)**

- Under **Key pair (login)**:

  - Choose an existing key pair (we can use the existing keys but it is not secure), or
  - Create a new key pair and download the `.pem` file.\
    This key is used to connect securely to your instance.

![[EC2 Key Pair.png]]

---

### **8. Configure Network Settings**

- Choose a **VPC** and **subnet**.
- Enable **Auto-assign Public IP** if you want internet access.
- Add **security group rules**:
  - For Linux: allow **SSH (port 22)**.
  - For Windows: allow **RDP (port 3389)**.
  - Add other ports (e.g., HTTP 80, HTTPS 443) if needed.

![[EC2 network settings.png]]

---

### **9. Configure Storage**

- Specify the **EBS volume size** (default is usually 8 GiB).
- Optionally, add more volumes.

![[EC2 Configure Storage.png]]

---

### **10. Review and Launch**

- Review all configurations.
- Click **Launch Instance**.
