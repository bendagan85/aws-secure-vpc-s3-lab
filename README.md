# AWS Cloud Security & Networking Portfolio ☁️🛡️

## 📌 Project Overview
This repository documents two advanced AWS lab scenarios simulating real-world production tasks.
1.  **Secure Private Network:** Designing a "bunker" architecture with isolated servers and private storage access.
2.  **Static Web Hosting:** Configuring a highly available serverless website accessible to the public.

---

# 🔒 Part 1: Secure VPC & PrivateLink Architecture

## 🎯 The Goal
To enable a **private EC2 instance** (isolated from the public internet) to securely upload sensitive data to an S3 bucket without using a NAT Gateway or Public IP, utilizing **AWS PrivateLink**.

## 🏗️ Architecture
* **VPC:** Custom VPC (`10.0.0.0/16`) segregated into Public and Private subnets.
* **Security Strategy:** Implemented a **Bastion Host** (Jump Server) architecture for secure SSH access.
* **Connectivity:** Established **VPC Peering** with the default VPC.
* **Private Link:** Deployed an **S3 Gateway Endpoint** to route storage traffic internally within the AWS backbone.

![Architecture Diagram](poc%20images/architecture-diagram.png)

## 🛠️ Implementation Steps
1.  **Network Setup:** Built custom VPC, Subnets, Internet Gateway, and Route Tables.
2.  **Access Control:** Configured Security Groups allowing SSH only from the Bastion Host.
3.  **Bastion Host:** Deployed a Public EC2 instance as the secure entry point.
4.  **Private Instance:** Deployed an Amazon Linux 2023 instance with **no Public IP**.
5.  **Secure Tunneling:** Configured VPC Gateway Endpoint for S3.

## 📸 Proof of Concept (POC)

### 1. Instance Isolation
The Private Instance has NO Public IP, while the Bastion Host does.
![EC2 Dashboard](poc%20images/instances.png)

### 2. Secure Access (SSH Jump)
Connection to Private IP (`10.0.2.x`) via Bastion Host using SSH Agent Forwarding.
![SSH Connection](poc%20images/proof%20of%20ssh%20to%20private%20ec2.png)

### 3. S3 Upload via PrivateLink
Successfully uploaded `secret.txt` to S3 from a server with NO internet access.
![S3 Upload Proof](poc%20images/proof%20of%20upload%20to%20s3%20no%20igw.png)

---

# 🌐 Part 2: S3 Static Website Hosting

## 🎯 The Goal
To deploy a public-facing static website using Amazon S3 without managing servers (Serverless architecture), focusing on public access configuration and bucket policies.

## 🛠️ Implementation Steps
1.  **Bucket Creation:** Created a dedicated bucket for web hosting (`ben-static-site-demo`).
2.  **Public Access:** Disabled "Block All Public Access" settings to allow external traffic.
3.  **Bucket Policy:** Configured a JSON policy granting `GetObject` permission to any user (`Principal: "*"`).
4.  **Hosting Config:** Enabled "Static website hosting" and mapped `index.html` as the entry document.

## 📸 Proof of Concept (POC)

### 1. The Live Site
Successfully accessed the static website via the public S3 endpoint URL.
![Static Website Proof](poc%20images/static-site-proof.png)

### 2. Security Configuration
Bucket Policy allowing public read access for the website assets.
![Bucket Policy](poc%20images/bucket-policy-config.png)

---

## 💻 Tech Stack
* **Cloud Provider:** AWS (VPC, EC2, S3, IAM, VPC Endpoints)
* **Networking:** Subnetting, Route Tables, Peering, Security Groups
* **OS:** Amazon Linux 2023, Ubuntu
* **Tools:** AWS CLI, SSH, Bash, JSON policies
