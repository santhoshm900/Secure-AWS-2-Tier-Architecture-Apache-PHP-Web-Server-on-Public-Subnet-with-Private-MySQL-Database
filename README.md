# Secure AWS 2-Tier Architecture: Apache-PHP Web Server on Public Subnet with Private MySQL Database

This project demonstrates a secure production-style 2-tier web application deployment on AWS.  
The web tier runs Apache and PHP on an EC2 instance in the public subnet, while the MySQL database is hosted in a private subnet to ensure isolation and security.

---
📌 Architecture Diagram
screenshots/vpc.jpeg


(Replace this with your uploaded architecture image)

🚀 Project Workflow

Below are the steps followed with screenshots.

1️⃣ Create VPC (10.0.0.0/16)

Create custom VPC

Enable DNS Resolution

📷 Screenshot:
/screenshots/vpc.jpeg

2️⃣ Create Subnets

Public Subnet: 10.0.0.0/24

Private Subnet: 10.0.1.0/24

📷 Screenshot:
/screenshots/subnets.jpeg

3️⃣ Create and Attach Internet Gateway

Create IGW

Attach to VPC

📷 Screenshot:
/screenshots/internet gatway.jpeg

4️⃣ Create NAT Gateway

Create NAT Gateway in public subnet

Assign Elastic IP

📷 Screenshot:
/screenshots/nat-gateway.jpeg

5️⃣ Create Route Tables
Public Route Table

Destination: 0.0.0.0/0 → IGW

📷 Screenshot:
/screenshots/Route-Table.jpeg

Private Route Table

Destination: 0.0.0.0/0 → NAT Gateway

📷 Screenshot:
/screenshots/prv-subnet-routre.jpeg

6️⃣ Security Groups
Web-SG

Inbound:

HTTP (80) → 0.0.0.0/0

SSH (22) → Your Public IP

📷 Screenshot:
/screenshots/web-inbount rule.jpeg

DB-SG

Inbound:

MySQL (3306) → Web-SG

SSH (22) → Web-SG / VPC

📷 Screenshot:
/screenshots/DB-inbound rule.jpeg

7️⃣ Launch EC2 (Web Server)

Ubuntu AMI

Public Subnet

Security Group = Web-SG

Key Pair = Linux-Keypair

📷 Screenshot:
/screenshots/IAAS-WEB.jpeg

8️⃣ Configure EC2 (Apache + PHP)

SSH into EC2:

sudo apt update
sudo apt install apache2 -y
sudo apt install php -y
sudo service apache2 restart


📷 Screenshot:
/screenshots/install apache services.jpeg

9️⃣ Create MySQL User & Database

SSH into DB EC2 or RDS and run:

sudo mysql -u root -p
CREATE USER 'appusr'@'%' IDENTIFIED BY 'sqluser2025';
GRANT ALL PRIVILEGES ON *.* TO 'appusr'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;


📷 Screenshot:
/screenshots/creat sql root acc.jpeg
/screenshots/grant privileges on sql claint.jpeg

🔟 Create Application Database
CREATE DATABASE appdb;
USE appdb;

CREATE TABLE Course(
  CourseID int,
  CourseName varchar(1000),
  Rating numeric(2,1)
);

INSERT INTO Course VALUES
(1,'AWS Certified Solutions Architect – Associate',4.5),
(2,'AWS Certified Solutions Architect – Professional',4.6),
(3,'AWS Certified DevOps Engineer – Professional',4.7);


📷 Screenshot:
/screenshots/application create in db.jpeg

1️⃣1️⃣ Upload Web Application Files

Using FileZilla:

Connect using EC2 public IP and Key-pair

Upload index.php → /var/www/html

📷 Screenshot:
/screenshots/FILEZILA insert html file.jpeg

1️⃣2️⃣ Web Server Access DB Privately

Test from Web EC2:

sudo mysql -h 10.0.1.xxx -u appusr -p


📷 Screenshot:
/screenshots/iaas-web access the db via http.jpeg

1️⃣3️⃣ Browser Access

Open:

http://<EC2-Public-IP>/index.php


Expected Output = Table:

AWS Certifications

Associate 4.5

Professional 4.6

DevOps 4.7

📷 Screenshot:
/screenshots/IAS-WEB pull the DB-application.jpeg

🎯 Final Output

EC2 Web Server ← private → MySQL database
DB is never exposed to public internet.

🛡️ Security Best Practices

No public IP on database

SSH restricted to specific IP

DB access allowed only from Web-SG

NAT used for private internet dependency

VPC isolation

Security Group micro-segmentation

📁 Repository Structure
.
├── README.md
└── screenshots/
    ├── vpc.jpeg
    ├── subnets.jpeg
    ├── internet gatway.jpeg
    ├── nat-gateway.jpeg
    ├── Route-Table.jpeg
    ├── prv-subnet-routre.jpeg
    ├── DB-inbound rule.jpeg
    ├── web-inbount rule.jpeg
    ├── IAAS-WEB.jpeg
    └── ...

⭐ Improvements (Future)

Use RDS MySQL instead of EC2

Add Application Load Balancer

Auto Scaling Group

S3 website assets

CloudWatch logging

Parameter Store for secrets

Terraform automation

👌 Conclusion

This project demonstrates real AWS infrastructure implementing enterprise-level security for a 2-tier application using:

VPC

Subnets

NAT

Route tables

EC2

MySQL

SSH & HTTP security
