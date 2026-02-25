# WordPress Deployment on AWS using EC2 + RDS + ALB + ASG (LAMP Stack)

---

## 1️⃣ Launch EC2 Instance

- AMI: Amazon Linux 2023
- Instance Type: t2.micro (Free Tier)
- Security Group:
  - SSH (22) – My IP
  - HTTP (80) – Anywhere
  - MySQL (3306) – From RDS Security Group (Recommended)

Connect to instance:

ssh -i key.pem ec2-user@<EC2-Public-IP>

---

## 2️⃣ Install MySQL Client

sudo dnf install -y mariadb105

Verify:

mysql --version

---

## 3️⃣ Create RDS MySQL Database

- Engine: MySQL
- Deployment: Single instance
- Public Access: Disabled
- Subnet: Private
- Attach Security Group allowing MySQL from EC2 SG

---

## 4️⃣ Connect to RDS from EC2

mysql -h <RDS-ENDPOINT> -P 3306 -u admin -p

---

## 5️⃣ Create WordPress Database & User

CREATE DATABASE wordpress;

CREATE USER 'wpuser' IDENTIFIED BY 'StrongPasswordHere';

GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser';

FLUSH PRIVILEGES;

EXIT;

---

## 6️⃣ Install Apache (HTTPD)

sudo dnf install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd

Verify:

systemctl status httpd

---

## 7️⃣ Install PHP

sudo dnf install -y php php-mysqlnd

Verify:

php -v

Restart Apache:

sudo systemctl restart httpd

---

## 8️⃣ Download WordPress

cd /home/ec2-user

wget https://wordpress.org/latest.tar.gz

tar -xzf latest.tar.gz

cd wordpress

---

## 9️⃣ Configure WordPress

Copy sample config:

cp wp-config-sample.php wp-config.php

Edit file:

vi wp-config.php

Update the following:

define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'StrongPasswordHere');
define('DB_HOST', '<RDS-ENDPOINT>');

---

## 🔐 Generate Secret Keys

Open in browser:

https://api.wordpress.org/secret-key/1.1/salt/

Copy generated keys and replace the AUTH_KEY section in wp-config.php.

---

## 🔟 Move WordPress Files to Web Root

sudo cp -r /home/ec2-user/wordpress/* /var/www/html/

Set permissions:

sudo chown -R apache:apache /var/www/html/

Restart Apache:

sudo systemctl restart httpd

---

## 1️⃣1️⃣ Access Application

Open browser:

http://<EC2-Public-IP>

Complete WordPress setup.

Admin panel:

http://<EC2-Public-IP>/wp-admin

---

# High Availability Setup

---

## 1️⃣ Create Target Group

- Type: Instances
- Protocol: HTTP
- Port: 80
- Health Check Path: /

---

## 2️⃣ Create Application Load Balancer

- Internet-facing
- Listener: HTTP (80)
- Attach Target Group

---

## 3️⃣ Create Launch Template

- Use working WordPress EC2 instance
- Select AMI created from instance
- Add user data if required

---

## 4️⃣ Create Auto Scaling Group

- Attach Launch Template
- Attach Target Group
- Desired Capacity: 2
- Min: 2
- Max: 4

---

## 5️⃣ Access via ALB

Use:

http://<ALB-DNS-NAME>

---

# Blue-Green Deployment Strategy

1. Create new Launch Template version
2. Update Auto Scaling Group
3. Gradually replace instances
4. Monitor health checks
5. Switch traffic safely

---

# Security Best Practices Followed

- RDS in private subnet
- No public DB access
- Restricted Security Groups
- IAM role-based access (recommended)
- No credentials stored in GitHub

---

# Future Enhancements

- Terraform automation
- CI/CD using Jenkins or GitHub Actions
- HTTPS using ACM
- CloudWatch monitoring
- Dockerized deployment
