# VPC Deployment Project

## Overview

<p align="justify">This project demonstrates the creation and configuration of a Virtual Private Cloud (VPC) on Amazon Web Services (AWS). A VPC is a virtual network that allows you to launch AWS resources in a defined virtual network environment. This README provides instructions on how to set up and configure a VPC along with additional subnets, routing tables, security groups, and cleanup procedures.</p>

## Project Steps

1. **Create a VPC:**
   - Create a Virtual Private Cloud (VPC) along with a public subnet. The public subnet is configured to have internet connectivity, allowing instances within it to communicate with the internet.

2. **Create Additional Subnets:**
   - Deploy services across multiple Availability Zones for high availability. Create a private subnet in Availability Zone B, different from the public subnet in Availability Zone A. This subnet remains private and uses the main route table.

3. **Edit the Routing Table:**
   - <p align="justify">Edit the route table association of the public subnet in Availability Zone B to match that of the public subnet in Availability Zone A. Ensure that both public subnets have access to the internet by confirming the presence of a route to the internet for the public subnet in Availability Zone B.</p>

4. **Create a Security Group:**
   - Create a security group for the VPC and configure inbound rules to allow HTTP and SSH traffic from your local IP address.

5. **Clean Up Resources:**
   - After completing the project, delete the VPC and associated resources.

## Repository Structure

The repository contains the following components:

- **`/VPC_Architecture`:**
  Contains the reference diagram illustrating the VPC architecture.
