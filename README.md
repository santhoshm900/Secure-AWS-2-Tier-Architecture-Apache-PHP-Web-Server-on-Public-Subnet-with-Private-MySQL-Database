# 🏗️ Secure AWS 2-Tier Architecture: Apache-PHP Web Server on Public Subnet with Private MySQL Database

This project demonstrates a secure **2-tier web application** deployment on AWS, where:

- **Apache + PHP Web Server** runs in a **public subnet**
- **MySQL Database** runs in a **private subnet**
- Database is never exposed to the internet
- Secure communication happens only through internal VPC network

The design follows AWS networking best-practices using **VPC, Subnets, NAT Gateway, Route Tables, and Security Groups**.

---

## 📌 Architecture Diagram

![Architecture Diagram](./screenshots/new-arc.drawio.png)

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
- HTTP (80) → `0.0.0.0/0`
- SSH (22) → Your Public IP  

📷 Screenshot:  
![Web SG Rules](./screenshots/web-inbound-rule.jpeg)

---

### 🔹 DB-SG (Database Server)
Inbound Rules:
- MySQL (3306) → Web-SG  
- SSH (22) → Web-SG / VPC  

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
```

📷 Screenshot:  
![Install Apache](./screenshots/install-apache-services.jpeg)

---

## 9️⃣ Create MySQL User & Permissions

Login to DB MySQL:

```sql
CREATE USER 'appusr'@'%' IDENTIFIED BY 'sqluser2025';
GRANT ALL PRIVILEGES ON *.* TO 'appusr'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

📷 Screenshots:  
![Create SQL Root](./screenshots/create-sql-root-acc.jpeg)  
![Grant Privileges](./screenshots/grant-privileges-on-sql-client.jpeg)

---

## 🔟 Create Database and Insert Sample Data

```sql
CREATE DATABASE appdb;
USE appdb;

CREATE TABLE Course(
  CourseID INT,
  CourseName VARCHAR(1000),
  Rating NUMERIC(2,1)
);

INSERT INTO Course VALUES
(1,'AWS Certified Solutions Architect – Associate',4.5),
(2,'AWS Certified Solutions Architect – Professional',4.6),
(3,'AWS Certified DevOps Engineer – Professional',4.7);
```

📷 Screenshot:  
![Create DB](./screenshots/application-create-in-db.jpeg)

---

## 1️⃣1️⃣ Upload Web Files using FileZilla

Upload `index.php` to:

```
/var/www/html
```

📷 Screenshot:  
![FileZilla Upload](./screenshots/filezilla-insert-html-file.jpeg)

---

## 1️⃣2️⃣ Test DB Access from Web Server

```bash
sudo mysql -h 10.0.1.xx -u appusr -p
```

📷 Screenshot:  
![Web DB Access](./screenshots/iaas-web-access-db.jpeg)

---

## 1️⃣3️⃣ Browser Test

Open in browser:

```
http://<EC2-Public-IP>/index.php
```

Expected Result → AWS Certification Table

📷 Screenshot:  
![Browser Test](./screenshots/iaas-web-access-db.jpeg)

---

# 🛡️ Security Highlights

- Database in **private subnet** (no public IP)
- Web server is the only entry point
- NAT used for secure outbound access
- Security Groups follow least-privilege model
- Full network isolation using VPC

---

## 📂 Repository Structure

```
Secure-AWS-2-Tier-Architecture/
│
├─ README.md
└─ screenshots/
      ├─ new-arc.drawio.png
      ├─ vpc.jpeg
      ├─ subnets.jpeg
      ├─ internet-gateway.jpeg
      ├─ nat-gateway.jpeg
      ├─ route-table.jpeg
      ├─ private-subnet-route.jpeg
      ├─ web-inbound-rule.jpeg
      ├─ db-inbound-rule.jpeg
      ├─ iaas-web.jpeg
      ├─ install-apache-services.jpeg
      ├─ create-sql-root-acc.jpeg
      ├─ grant-privileges-on-sql-client.jpeg
      ├─ application-create-in-db.jpeg
      ├─ filezilla-insert-html-file.jpeg
      └─ iaas-web-access-db.jpeg
```

---

## ✅ Conclusion

This architecture provides a secure, scalable and production-ready environment for deploying web applications on AWS with strict network isolation.


