# AWS 3-Tier Architecture Deployment Guide

This guide provides step-by-step instructions for deploying a 3-tier architecture on AWS. The architecture organizes applications into three logical and physical computing tiers:

- **Interface / Presentation Tier**: Responsible for handling user interactions and displaying data.
- **Application Tier**: Where data processing occurs, including business logic implementation.
- **Data Tier**: Stores and manages data associated with the application.

## Architecture Overview

- **1-tier**: Public subnets in 2 availability zones.
- **2-tier**: Private subnets in 2 availability zones.
- **3-tier**: Private subnets in 2 availability zones.

## Deployment Steps

1. **Select Region**: Choose the AWS region where you want to create your VPC.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/b56d4b35-34d7-45de-a166-4337f218f143)

2. **Create VPC**: Deploy a Virtual Private Cloud (VPC). This automatically creates a default private route table.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/67ee5b98-735c-434f-84b7-3ea000c6b7f2)

3. **Internet Gateway**: Create and attach an Internet Gateway to the VPC to enable internet access for instances.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/16ebd6ce-c8c8-4525-b73a-989f86447812)

4. **Create Public Subnets**: Establish two public subnets in different availability zones. Modify IP settings to enable auto-assignment of IPv4 addresses.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/4e9e2812-d05e-4ba9-ad32-6737cb378f19)

5. **Configure Public Route Table**: Create a public route table and add a route for all traffic (`0.0.0.0/0`) via the Internet Gateway. Associate this route table with public subnets.

6. **Create Private Subnets**: Set up four private subnets (2 in each AZ) for application and database tiers.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/c447469a-4052-4186-a819-1dcd88ce2186)

7. **Private Route Tables**: Create two route tables for private subnets and associate them accordingly.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/3e3cdcd9-fb4f-4499-942b-b199b9cce29b)

8. **NAT Gateway**: Deploy NAT Gateways in public subnets. Allocate two Elastic IP addresses for high availability. Update private route tables to direct internet-bound traffic through NAT Gateways.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/5705a36d-91e5-4286-be62-cafe1e4544e6)

9. **Testing Outbound Access**: Launch EC2 instances in private subnets (application tier) and test outbound internet access using AWS Systems Manager Run Command.

   a. Create IAM Role for EC2 instances to use Run Command (System Manager).
   
   b. Launch EC2 instances in private subnets and attach IAM roles.
   
   c. Use Run Command to test outbound internet access (`sudo yum update -y`).
   
   d. Repeat the process for EC2 instances in the second AZ.

![image](https://github.com/DDMateus/AWS-3-Tier-Architecture/assets/88774178/1ce77fad-8972-4476-a366-3f1a3c94f138)

## Testing

Verify successful outbound internet access through NAT Gateways by testing updates and security patches download on EC2 instances.

## Repository Structure

The repository is structured as follows:

- **`/AWS_3_Tier_Architecture`:** Contains the reference diagram illustrating the application architecture.

## Conclusion

By following these deployment steps, you can establish a secure and scalable 3-tier architecture on AWS, ensuring reliable internet connectivity for your applications while maintaining robust security measures.

