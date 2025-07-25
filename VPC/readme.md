#AWS VPC Architecture with ALB, Auto Scaling Group, NAT Gateway & Bastion Host

![AWS VPC Architecture](vpc.jpeg)

This documentation explains step-by-step how to create a **highly available VPC setup** with:

* 2 Availability Zones (AZs)
* Public and Private Subnets
* NAT Gateways for outbound internet access
* Auto Scaling Group in private subnet (with Launch Template)
* Bastion Host for SSH access
* Application Load Balancer (ALB) in public subnet serving traffic to private instances

---

## ✅ Architecture Overview

* **VPC** with 2 AZs
* **Public Subnets** (one in each AZ) → Hosts NAT Gateways & ALB
* **Private Subnets** (one in each AZ) → Hosts EC2 instances via Auto Scaling Group
* **Bastion Host** in public subnet for SSH access to private instances
* **Security Groups** for controlled access
* **Application Load Balancer** on port 80 → Targets private EC2 instances on port 8000

---

## 🛠 Step 1: Create VPC

1. Go to **VPC > Create VPC**
2. Name: MyVPC
3. IPv4 CIDR: 10.0.0.0/16
4. Create VPC

---

## 🛠 Step 2: Create Subnets

* Create **4 Subnets**:

  * **Public Subnet 1**: 10.0.1.0/24 in AZ1
  * **Public Subnet 2**: 10.0.2.0/24 in AZ2
  * **Private Subnet 1**: 10.0.3.0/24 in AZ1
  * **Private Subnet 2**: 10.0.4.0/24 in AZ2

Enable **Auto-assign public IP** for Public Subnets only.

---

## 🛠 Step 3: Create Internet Gateway & Route Tables

1. Create **Internet Gateway** and attach to VPC
2. Public Route Table:

   * Add route 0.0.0.0/0 → Internet Gateway
   * Associate with Public Subnets
3. Private Route Table:

   * Will route through NAT Gateway later

---

## 🛠 Step 4: Create NAT Gateways

1. Allocate 2 Elastic IPs
2. Create NAT Gateways:

   * **NAT Gateway 1** in Public Subnet 1
   * **NAT Gateway 2** in Public Subnet 2
3. Update **Private Route Table**:

   * Default route 0.0.0.0/0 → NAT Gateway (use AZ-specific routing for HA)

---

## 🛠 Step 5: Create Security Groups

* **ALB Security Group**:

  * Inbound: HTTP (80) from Anywhere (0.0.0.0/0)
  * Outbound: All traffic

* **Private EC2 Security Group**:

  * Inbound: Port 8000 from ALB SG, SSH from Bastion SG
  * Outbound: All traffic

* **Bastion Host Security Group**:

  * Inbound: SSH (22) from your IP
  * Outbound: All traffic

---

## 🛠 Step 6: Launch Bastion Host (Jump Server)

1. Launch EC2 in **Public Subnet**
2. Associate **Bastion SG**
3. SSH into Bastion:

bash
scp -i ec2-new.pem ec2-new.pem ec2-user@<Bastion-Public-IP>:/home/ec2-user/
ssh -i ec2-new.pem ec2-user@<Bastion-Public-IP>


---

## 🛠 Step 7: Create Launch Template & Auto Scaling Group

1. Create **Launch Template**:

   * AMI: Amazon Linux 2
   * Instance Type: t2.micro
   * Security Group: Private EC2 SG
   * **Do NOT enable public IP**
   * User Data:

bash
#!/bin/bash
yum update -y
yum install python3 -y
echo "<html><h1>My Pvt Subnet $(hostname)</h1></html>" > /home/ec2-user/index.html


2. Create **Auto Scaling Group**:

   * Desired Capacity: 2
   * Min: 2, Max: 4
   * Subnets: Private Subnets only

---

## 🛠 Step 8: Connect via Bastion & Start Python Server

1. From Bastion:

bash
ssh -i ec2-new.pem ec2-user@<Private-Instance-IP>
cd /home/ec2-user
python3 -m http.server 8000


* In Instance 1: Change <h1> text to My Pvt Subnet 1
* In Instance 2: Change <h1> text to My Pvt Subnet 2

---

## 🛠 Step 9: Create Application Load Balancer

1. Go to **EC2 > Load Balancers > Create ALB**
2. Name: MyAppALB
3. Scheme: Internet-facing
4. Listeners: HTTP on port 80
5. Subnets: Public Subnets
6. Security Group: ALB SG

### Target Group

* Name: my-target-group
* Target type: Instances
* Protocol: HTTP
* Port: 8000
* Register the 2 private instances

---

## ✅ Test Setup

* Access http://<ALB-DNS-Name>
* Refresh to see content from different private subnets

---

## 🔒 Security Group Recap

* **ALB SG**: Allows HTTP from the internet
* **Private SG**: Allows 8000 from ALB SG, SSH from Bastion SG
* **Bastion SG**: Allows SSH from your IP

---

## ✅ Recap of Architecture

* VPC with 2 AZs, Public and Private Subnets
* NAT Gateways for outbound internet from private subnets
* Auto Scaling Group in private subnet (desired=2, max=4)
* Bastion host for secure SSH
* ALB distributing traffic to private instances on port 8000

---
