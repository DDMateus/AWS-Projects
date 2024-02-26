## Hosting a Static Website on AWS

### Overview
<p align="justify">This project demonstrates the deployment of a static HTML web application on Amazon Web Services (AWS). The project utilizes various AWS resources and services to host the web app securely and efficiently.</p>

![StaticWebsiteArchitecture](https://github.com/DDMateus/AWS-Projects/assets/88774178/535aa5f0-df33-4af8-ae2b-a35ebb3dcf2d)

### Resources Used
1. **Virtual Private Cloud (VPC)**:
   - Configured a VPC with public and private subnets across two availability zones for enhanced fault tolerance and reliability.

2. **Internet Gateway**:
   - Deployed an Internet Gateway to facilitate connectivity between VPC instances and the internet.

3. **Security Groups**:
   - Established Security Groups as a network firewall mechanism.

4. **Availability Zones**:
   - Utilized two Availability Zones to enhance system reliability and fault tolerance.

5. **Subnets**:
   - Utilized Public Subnets for infrastructure components like the NAT Gateway and Application Load Balancer (ALB).
   - Positioned web servers (EC2 instances) within Private Subnets for enhanced security.

6. **EC2 Instance Connect Endpoint**:
   - Implemented EC2 Instance Connect Endpoint for secure connections to assets within both public and private subnets.

7. **NAT Gateway**:
   - Enabled instances in private subnets to access the internet via the NAT Gateway.

8. **Web Servers (EC2 Instances)**:
   - Hosted the website on EC2 instances deployed within the private subnets.

9. **Application Load Balancer (ALB)**:
   - Employed an Application Load Balancer and a target group to evenly distribute web traffic to an Auto Scaling Group of EC2 instances across multiple Availability Zones.

10. **Auto Scaling Group**:
    - Utilized an Auto Scaling Group to automatically manage EC2 instances, ensuring website availability, scalability, fault tolerance, and elasticity.

11. **GitHub**:
    - Stored web files on GitHub for version control and collaboration.

12. **Certificate Manager**:
    - Secured application communications using a Certificate Manager.

13. **Simple Notification Service (SNS)**:
    - Configured Simple Notification Service (SNS) to alert about activities within the Auto Scaling Group.

14. **Route 53**:
    - Registered the domain name and set up a DNS record using Route 53.

### Deployment Script
```bash
#!/bin/bash
# Switch to the root user to gain full administrative privileges
sudo su
# Update all installed packages to their latest versions
yum update -y
# Install Apache HTTP Server
yum install -y httpd
# Change the current working directory to the Apache web root
cd /var/www/html
# Install Git
yum install git -y
# Clone the project GitHub repository to the current directory
git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git
# Copy all files, including hidden ones, from the cloned repository to the Apache web root
cp -R host-a-static-website-on-aws/. /var/www/html/
# Remove the cloned repository directory to clean up unnecessary files
rm -rf host-a-static-website-on-aws
# Enable the Apache HTTP Server to start automatically at system boot
systemctl enable httpd
# Start the Apache HTTP Server to serve web content
systemctl start httpd
```

### Conclusion
<p align="justify">By following the outlined steps and utilizing AWS services, the project successfully hosts a static website securely, with scalability, fault tolerance, and high availability. The provided deployment script automates the setup process, making it easier to replicate the deployment in different environments.</p>
