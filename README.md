# WordPress on AWS – LAMP Stack with RDS, ALB & Auto Scaling

## 🚀 Project Overview

Deployed a production-ready WordPress application on AWS using:

- EC2 (Amazon Linux 2023)
- Apache (httpd)
- PHP 8.x
- RDS MySQL (Private subnet)
- Application Load Balancer (ALB)
- Auto Scaling Group (ASG)
- Blue/Green Deployment Strategy

---

## 🏗 Architecture Flow

Client → ALB → Target Group → EC2 Instances → RDS (Private)

---

## ⚙️ Implementation Steps

### 1️⃣ EC2 Setup
- Launched Amazon Linux 2023
- Installed LAMP stack
- Configured Apache & PHP

### 2️⃣ RDS Setup
- Created MySQL RDS instance (Private)
- Disabled public access
- Configured security group

### 3️⃣ Database Configuration
- Created `wordpress` database
- Created DB user and granted privileges

### 4️⃣ WordPress Installation
- Downloaded latest WordPress
- Configured `wp-config.php`
- Added secret keys
- Connected to RDS endpoint

### 5️⃣ High Availability Setup
- Created Target Group
- Configured Application Load Balancer
- Created Launch Template
- Created Auto Scaling Group

---

## 🔐 Security

- RDS in private subnet
- MySQL access restricted via SG
- SSH restricted
- No hardcoded credentials in repository

---

## 📌 Future Improvements

- Terraform Infrastructure as Code
- CI/CD using Jenkins
- HTTPS via ACM
- Monitoring via CloudWatch
- Dockerized WordPress deployment

---

👨‍💻 Built by Yashwanth Kumar
DevOps Engineer | AWS | Kubernetes | CI/CD
