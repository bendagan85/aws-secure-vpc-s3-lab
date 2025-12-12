# AWS Secure VPC & PrivateLink Lab 🛡️

## 📌 Project Overview
This project demonstrates a secure cloud network architecture designed to simulate a production environment. 
The primary goal was to enable a **private EC2 instance** (isolated from the public internet) to securely upload sensitive data to an S3 bucket without using a NAT Gateway or Public IP, utilizing **AWS PrivateLink**.

## 🏗️ Architecture
* **VPC:** Custom VPC (`10.0.0.0/16`) segregated into Public and Private subnets.
* **Security Strategy:** Implemented a **Bastion Host** (Jump Server) architecture for secure SSH access to internal resources.
* **Connectivity:** Established **VPC Peering** with the default VPC for cross-network communication.
* **Private Link:** Deployed an **S3 Gateway Endpoint** to route storage traffic internally within the AWS backbone, bypassing the public internet entirely.

![Architecture Diagram](architecture-diagram.png)

## 🛠️ Implementation Steps
1. **Network Infrastructure:** Built a custom VPC with Public and Private subnets, Internet Gateway, and custom Route Tables.
2. **Access Control:** Configured Security Groups to strictly allow SSH traffic only from the Bastion Host to the Private Instance.
3. **Bastion Host:** Deployed a Public EC2 instance to serve as the secure entry point.
4. **Private Instance:** Deployed an Amazon Linux 2023 instance in the private subnet with **no Public IP**.
5. **Secure Tunneling:** Configured a VPC Gateway Endpoint for S3 and updated the Private Route Table to route S3 traffic through the endpoint.

## 📸 Proof of Concept (POC)

### 1. Instance Isolation
Demonstrating the security posture: The Private Instance has NO Public IP address, while the Bastion Host does.
![EC2 Dashboard](instances.png)

### 2. Secure Access (SSH Jump)
Successfully connected to the Private IP (`10.0.2.x`) via the Bastion Host using SSH Agent Forwarding / Key management.
![SSH Connection](proof%20of%20ssh%20to%20private%20ec2.png)

### 3. S3 Upload via PrivateLink
**The Challenge:** Upload a file to S3 from a server with NO internet access.
**The Result:** Successfully uploaded `secret.txt` via the VPC Endpoint.
![S3 Upload Proof](proof%20of%20upload%20to%20s3%20no%20igw.png)

## 💻 Tech Stack
* **Cloud Provider:** AWS (VPC, EC2, S3, IAM, VPC Endpoints)
* **OS:** Amazon Linux 2023, Ubuntu
* **Tools:** AWS CLI, SSH, Bash
