# 🏗️ Secure AWS 2-Tier Architecture: Apache-PHP Web Server on Public Subnet with Private MySQL Database

This project demonstrates a secure **2-tier web application** deployment on AWS, where:

- **Apache + PHP Web Server** runs in a **public subnet**
- **MySQL Database** runs in a **private subnet**
- Database is **never exposed to the internet**
- Secure communication happens only through internal VPC traffic

The design follows AWS networking best practices using **VPC, Subnets, NAT Gateway, Route Tables, and Security Groups**.

---

## 📚 Table of Contents

- [Architecture Diagram](#-architecture-diagram)
- [Project Workflow](#-project-workflow)
  - [Create VPC](#1️⃣-create-vpc-10000016)
  - [Create Subnets](#2️⃣-create-subnets)
  - [Internet Gateway](#3️⃣-create-and-attach-internet-gateway)
  - [NAT Gateway](#4️⃣-create-nat-gateway)
  - [Route Tables](#5️⃣-create-route-tables)
  - [Security Groups](#6️⃣-configure-security-groups)
  - [Launch Web Server](#7️⃣-launch-ec2-web-server-apachephp)
  - [Install Apache & PHP](#8️⃣-install-apache--php)
  - [Database Setup](#9️⃣-create-mysql-user--permissions)
  - [Application Setup](#-create-database-and-insert-sample-data)
  - [Upload Web Files](#1️⃣1️⃣-upload-web-files-using-filezilla)
  - [Test DB Access](#1️⃣2️⃣-test-db-access-from-web-server)
  - [Browser Output](#1️⃣3️⃣-browser-test)
- [Security Highlights](#-security-highlights)
- [Repository Structure](#-repository-structure)
- [Conclusion](#-conclusion)

---

## 📌 Architecture Diagram

![Architecture Diagram](../screenshots/new-arc.drawio.jpng)

---

# 🚀 Project Workflow

Below are the complete steps followed with screenshots.

---

## 1️⃣ Create VPC (10.0.0.0/16)

- Create custom VPC  
- Enable DNS Resolution  

📷 Screenshot:  
![VPC Creation](./screenshots/vpc.jpeg)

---

## 2️⃣ Create Subnets

- **Public Subnet:** `10.0.0.0/24`  
- **Private Subnet:** `10.0.1.0/24`

📷 Screenshot:  
![Subnets](./screenshots/subnets.jpeg)

---

## 3️⃣ Create and Attach Internet Gateway

- Create Internet Gateway  
- Attach IGW to VPC  

📷 Screenshot:  
![Internet Gateway](./screenshots/internet-gateway.jpeg)

---

## 4️⃣ Create NAT Gateway

- Create NAT Gateway in Public Subnet  
- Assign Elastic IP  

📷 Screenshot:  
![NAT Gateway](./screenshots/nat-gateway.jpeg)

---

## 5️⃣ Create Route Tables

### 🔹 Public Route Table
- `0.0.0.0/0` → Internet Gateway  

📷 Screenshot:  
![Route Table](./screenshots/route-table.jpeg)

---

### 🔹 Private Route Table
- `0.0.0.0/0` → NAT Gateway  

📷 Screenshot:  
![Private Route Table](./screenshots/private-subnet-route.jpeg)

---

## 6️⃣ Configure Security Groups

### 🔹 Web-SG (Web Server)

Inbound Rules:

| Type | Port | Source |
|------|------|--------|
| HTTP | 80 | 0.0.0.0/0 |
| SSH  | 22 | Your Public IP |

📷 Screenshot:  
![Web SG Rules](./screenshots/web-inbound-rule.jpeg)

---

### 🔹 DB-SG (Database Server)

Inbound Rules:

| Type | Port | Source |
|------|------|--------|
| MySQL | 3306 | Web-SG |
| SSH   | 22   | Web-SG |

📷 Screenshot:  
![DB SG Rules](./screenshots/db-inbound-rule.jpeg)

---

## 7️⃣ Launch EC2 Web Server (Apache+PHP)

- AMI: Ubuntu  
- Subnet: Public  
- SG: Web-SG  
- Key Pair: Linux-Keypair  

📷 Screenshot:  
![Web EC2](./screenshots/iaas-web.jpeg)

---

## 8️⃣ Install Apache & PHP

SSH into Web EC2:

```bash
sudo apt update
sudo apt install apache2 -y
sudo apt install php libapache2-mod-php php-mysql -y
sudo systemctl restart apache2
