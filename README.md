# Secure AWS 2-Tier Architecture: Apache-PHP Web Server on Public Subnet with Private MySQL Database

This project demonstrates a secure production-style 2-tier web application deployment on AWS.  
The web tier runs Apache and PHP on an EC2 instance in the public subnet, while the MySQL database is hosted in a private subnet to ensure isolation and security.

---

## 🏗️ Architecture Overview

**Design Pattern:** 2-Tier Application  
- **Tier-1 (Web):** Apache + PHP on EC2 (Public Subnet)  
- **Tier-2 (Database):** MySQL (Private Subnet)  

**Network Design**
- **VPC CIDR:** 10.0.0.0/16  
- **Subnets:**
  - Public Subnet: 10.0.0.0/24
  - Private Subnet: 10.0.1.0/24
- **Internet Access:**
  - Internet Gateway (Public Subnet)
  - NAT Gateway (Outbound access for Private Subnet)
- **Routing:**
  - Public RT → IGW for 0.0.0.0/0
  - Private RT → NAT for 0.0.0.0/0

---

## 🔐 Security Design
- Database has **no public IP**
- MySQL port **3306 allowed only from Web Security Group**
- EC2 SSH access restricted
- Least-privilege Security Group approach
- Outbound internet via NAT for DB patching

---

## 🧭 AWS Services Used
- Amazon VPC
- Subnets (Public & Private)
- Internet Gateway
- NAT Gateway
- Route Tables
- EC2 Instance
- MySQL Database
- Security Groups
- Elastic IP
- SFTP (FileZilla)

---

## 🚀 Deployment Steps (Overview)

1. Create VPC (10.0.0.0/16)
2. Create public and private subnets
3. Attach Internet Gateway to VPC
4. Configure NAT Gateway for private subnet
5. Create Route Tables and associate subnets
6. Launch EC2 instance in public subnet
7. Install Apache & PHP
8. Install and configure MySQL in private subnet
9. Create MySQL user and database
10. Deploy PHP code via SFTP (FileZilla)
11. Test DB connectivity from web application

---

## 💻 Tech Stack

| Component | Technology |
|----------|-------------|
| Compute | EC2 (Ubuntu) |
| Web Server | Apache |
| Language | PHP |
| Database | MySQL |
| Networking | VPC, IGW, NAT, RT |
| Security | Security Groups |

---

## 📷 Screenshots

> See `/screenshots` folder for:
- VPC & Subnets
- Route tables
- IGW & NAT
- EC2 instances
- MySQL DB
- Web Application output

---

## 🧪 Output

Sample Web UI:  
