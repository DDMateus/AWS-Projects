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

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/4045ef85-3f29-44c4-88f7-8ce11bfb267c)

2. **Create VPC**: Deploy a Virtual Private Cloud (VPC). This automatically creates a default private route table.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/06370fba-c88a-4cc8-b614-ed72f33485a1)

3. **Internet Gateway**: Create and attach an Internet Gateway to the VPC to enable internet access for instances.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/8d294f76-b659-422a-bbe1-ef0ad872a39c)

4. **Create Public Subnets**: Establish two public subnets in different availability zones. Modify IP settings to enable auto-assignment of IPv4 addresses.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/b7efdddf-1715-4751-b88b-53ee1f0f3ff1)

5. **Configure Public Route Table**: Create a public route table and add a route for all traffic (`0.0.0.0/0`) via the Internet Gateway. Associate this route table with public subnets.

6. **Create Private Subnets**: Set up four private subnets (2 in each AZ) for application and database tiers.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/2b173d07-57b8-4955-ba6a-aabdcf85f168)

7. **Private Route Tables**: Create two route tables for private subnets and associate them accordingly.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/8c92c266-4eb2-4995-99ed-2f7d7723fe2f)

8. **NAT Gateway**: Deploy NAT Gateways in public subnets. Allocate two Elastic IP addresses for high availability. Update private route tables to direct internet-bound traffic through NAT Gateways.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/6e45011c-c52a-402f-be82-7719757b992b)

9. **Testing Outbound Access**: Launch EC2 instances in private subnets (application tier) and test outbound internet access using AWS Systems Manager Run Command.

   a. Create IAM Role for EC2 instances to use Run Command (System Manager).
   
   b. Launch EC2 instances in private subnets and attach IAM roles.
   
   c. Use Run Command to test outbound internet access (`sudo yum update -y`).
   
   d. Repeat the process for EC2 instances in the second AZ.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/63e026dd-9f79-4868-8f02-8c5dd305ed1d)

## Testing

Verify successful outbound internet access through NAT Gateways by testing updates and security patches download on EC2 instances.

## Repository Structure

The repository is structured as follows:

- **`/AWS_3_Tier_Architecture`:** Contains the reference diagram illustrating the application architecture.

## Conclusion

By following these deployment steps, you can establish a secure and scalable 3-tier architecture on AWS, ensuring reliable internet connectivity for your applications while maintaining robust security measures.

