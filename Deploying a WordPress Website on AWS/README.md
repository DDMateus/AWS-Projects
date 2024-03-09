# Deploying a WordPress Website on AWS

<p align="justify">In this guide, we will walk through the process of deploying a WordPress website on AWS using a 3-tier architecture. The architecture consists of the following tiers:</p>

1. **First Tier (Public Subnet):**
   - This tier includes the NAT Gateway, Bastion Host, and Application Load Balancer (ALB).

2. **Second Tier (Private Subnet):**
   - This tier houses the web servers (EC2 instances).

3. **Third Tier (Private Subnet):**
   - This tier contains the database instances.

## Process Overview

1. **Create a VPC with Public and Private Subnets:**
   - Configure two Availability Zones and enable DNS Hostname in the VPC.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/0bf0384a-31e7-43cf-8b89-115544a175da)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/48617a1b-9b8e-4674-b19c-bb075078eb4e)

2. **Set Up Internet Gateway:**
   - Create and attach an Internet Gateway to the VPC for communication between instances in the VPC and the Internet.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/db8db765-25d1-4ed5-a0fa-4579adfd8f01)

3. **Create Public Subnets:**
   - Create two public subnets in each Availability Zone and enable auto-assign public IPv4 addresses.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/4bf029f9-f7ca-4ae3-8490-62b0099a9240)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/1bfbeaa2-34f8-4636-b118-38102b197e6a)

4. **Configure Public Route Tables:**
   - Associate public subnets with a public route table and add a route to direct traffic to the Internet Gateway.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/041741d4-262a-4537-a3b0-d7aded639926)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/3442eaec-f434-4f4c-bbfc-3d9d7e03e8e9)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/2a1414e9-4bc2-4d55-a471-b724fefe7fe0)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/479ef7eb-8370-44d2-abc2-73515f5e2fc8)

5. **Create Private Subnets:**
   - Repeat the subnet creation process for private subnets in each Availability Zone.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/375371f5-8bf7-4517-b498-fd72782e5f00)

6. **Set Up NAT Gateways:**
   - Create NAT Gateways in each public subnet to allow private subnet instances to access the Internet.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d899e853-a919-45cd-9420-84ffce67732e)

7. **Configure Private Route Tables:**
   - Associate private subnets with private route tables and add routes to direct traffic to the respective NAT Gateways.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/e4c490f8-8380-4c5f-9cdf-b1e981566c70)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/fb5a6cc8-6770-4dd9-8865-0c00c812a66b)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/c4322f84-017b-4868-a7e5-43b22481edb0)

Association for one availability zone:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/79cce2f9-17ef-4b2c-8c50-f7e15c47a4a8)

8. **Create Security Groups:**
   - Configure security groups for the Application Load Balancer, SSH access, database, web servers, and Elastic File System (EFS).

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/7a486a03-34ad-4c72-8015-d2b713594e5e)

9. **Set Up RDS Instance:**
   - Create a MySQL RDS database instance, configure subnet groups, and save credentials for database access.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/6871694e-8a25-486f-95ce-ac49482fa7fe)

10. **Deploy Elastic File System (EFS):**
    - Create an EFS to share files across web servers in private subnets.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/63309704-4085-488f-ae1d-0e416446b895)

11. **Generate Key Pair (Windows users):**
    - Create a key pair for SSH access to EC2 instances.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/16929c85-849c-485a-ba91-f1820a886877)

12. **Launch EC2 Instance for WordPress Installation:**
    - Launch an EC2 instance in the public subnet, install WordPress, and move files to EFS.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/a8e0879e-3729-443e-941a-5e16c91d2762)

13. **SSH into EC2 Instances (Windows users):**
    - Use PuTTY to SSH into EC2 instances for setup and configuration.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/b219dbdf-8836-4aec-bfc7-3c99ac1f5270)

14. **Install WordPress on EC2 Instance:**
    - Install Apache, PHP, MySQL, and WordPress on the EC2 instance.

```bash
#1. create the html directory and mount the efs to it
sudo su
yum update -y
mkdir -p /var/www/html
sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-06b9d3f972db91fc3.efs.us-east-1.amazonaws.com:/ /var/www/html

#2. install apache 
sudo yum install -y httpd httpd-tools mod_ssl
sudo systemctl enable httpd 
sudo systemctl start httpd

#3. install php 7.4
sudo amazon-linux-extras enable php7.4
sudo yum clean metadata
sudo yum install php php-common php-pear -y
sudo yum install php-{cgi,curl,mbstring,gd,mysqlnd,gettext,json,xml,fpm,intl,zip} -y

#4. install mysql5.7
sudo rpm -Uvh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
sudo yum install mysql-community-server -y
sudo systemctl enable mysqld
sudo systemctl start mysqld

#5. set permissions
sudo usermod -a -G apache ec2-user
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
sudo find /var/www -type f -exec sudo chmod 0664 {} \;
chown apache:apache -R /var/www/html 

#6. download wordpress files
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
cp -r wordpress/* /var/www/html/

#7. create the wp-config.php file
cp /var/www/html/wp-config-sample.php /var/www/html/wp-config.php

#8. edit the wp-config.php file
nano /var/www/html/wp-config.php

#9. restart the webserver
service httpd restart
```

We have succefully installed WordPress and moved the files into EFS.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/640b08be-ed66-4e9f-969a-e0cc758073ec)

15. **Set Up Application Load Balancer:**
    - Create an ALB to route traffic to EC2 instances in private subnets.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/24734e1e-eaa0-44cb-b2c6-f2f70befd69e)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/2f048d54-754b-4964-aa36-66466b10bdbb)

Access the website with application load balancer DNS name.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/e1e490c7-db52-4dca-9b88-1901b72c1a08)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/54ff3e59-82e7-48f1-92b6-a1af040f4f70)

16. **Register Domain Name and Create DNS Records:**
    - Register a domain name and configure Route 53 to map the domain to the ALB.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/7aa1b23a-24fc-4a61-9fd3-ed376dd358b7)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/4b0a75f0-3c5c-4589-8f69-a63dc8697f25)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/7f3bc08f-9102-4a0b-9c9e-a38561f3cc8c)

(problems loading the image)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/408db8fe-18db-4135-941e-b2c09015b400)


17. **Obtain SSL Certificate from AWS Certificate Manager:**
    - Secure website communication by obtaining and configuring an SSL certificate.

18. **Launch Bastion Host:**
    - Create an EC2 instance in the public subnet to facilitate SSH access to instances in private subnets.

19. **SSH into Instances in Private Subnet (Windows Users):**
    - SSH into the Bastion Host and from there, into instances in private subnets using their private IP.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/4d952f3e-40d9-4327-8253-46279e651716)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/965a552f-87af-46c4-8778-c3960272f490)

20. **Configure HTTPS Listener for ALB:**
    - Enable HTTPS listener for the ALB to secure website traffic and edit the HTTP listener to redirect traffic to HTTPS listener.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/cf9f7057-9069-4aa7-b79e-5fe936fdf5c3)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/5513af8e-6fa1-4288-b6ca-9c411845379b)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/42675472-eb9d-4aa3-95b4-caa1370016a0)

21. **Create Auto Scaling Group:**
    - Set up Auto Scaling to maintain EC2 instances and ensure high availability and scalability.

Instances launched using Auto Scaling Group.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d556eede-bbba-41d9-b76d-cf3acd82d3d3)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/c6c47e48-2828-48a2-ab7e-56a213a8cfbb)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/003103b2-e83b-44b8-bbb6-e8b8663e223e)

Architecture:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/49faa4e4-bfcf-49f2-a8a9-2176810a4e76)
