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

2. **Set Up Internet Gateway:**
   - Create and attach an Internet Gateway to the VPC for communication between instances and the Internet.

3. **Create Public Subnets:**
   - Create two public subnets in each Availability Zone and enable auto-assign public IPv4 addresses.

4. **Configure Public Route Tables:**
   - Associate public subnets with a public route table and add a route to direct traffic to the Internet Gateway.

5. **Create Private Subnets:**
   - Repeat the subnet creation process for private subnets in each Availability Zone.

6. **Set Up NAT Gateways:**
   - Create NAT Gateways in each public subnet to allow private subnet instances to access the Internet.

7. **Configure Private Route Tables:**
   - Associate private subnets with private route tables and add routes to direct traffic to the respective NAT Gateways.

8. **Create Security Groups:**
   - Configure security groups for the Application Load Balancer, SSH access, database, web servers, and Elastic File System (EFS).

9. **Set Up RDS Instance:**
   - Create a MySQL RDS database instance, configure subnet groups, and save credentials for database access.

10. **Deploy Elastic File System (EFS):**
    - Create an EFS to share files across web servers in private subnets.

11. **Generate Key Pair (Windows users):**
    - Create a key pair for SSH access to EC2 instances.

12. **Launch EC2 Instance for WordPress Installation:**
    - Launch an EC2 instance in the public subnet, install WordPress, and move files to EFS.

13. **SSH into EC2 Instances (Windows users):**
    - Use PuTTY to SSH into EC2 instances for setup and configuration.

14. **Install WordPress on EC2 Instance:**
    - Install Apache, PHP, MySQL, and WordPress on the EC2 instance.

15. **Set Up Application Load Balancer:**
    - Create an ALB to route traffic to EC2 instances in private subnets.

16. **Register Domain Name and Create DNS Records:**
    - Register a domain name and configure Route 53 to map the domain to the ALB.

17. **Obtain SSL Certificate from AWS Certificate Manager:**
    - Secure website communication by obtaining and configuring an SSL certificate.

18. **Launch Bastion Host:**
    - Create an EC2 instance in the public subnet to facilitate SSH access to instances in private subnets.

19. **SSH into Instances in Private Subnet (Windows Users):**
    - SSH into the Bastion Host and from there, into instances in private subnets.

20. **Configure HTTPS Listener for ALB:**
    - Enable HTTPS listener for the ALB to secure website traffic.

21. **Create Auto Scaling Group:**
    - Set up Auto Scaling to maintain EC2 instances and ensure high availability and scalability.
