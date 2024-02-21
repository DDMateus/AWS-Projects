## VPC Peering Connection Setup Guide

### Overview
A VPC peering connection establishes a private network connection between two VPCs, allowing secure communication between them. In this guide, we'll peer VPC A with VPC B and VPC A with VPC C using CloudFormation templates.

### Architecture
- **Peering VPC A with VPC B**: Enables communication between instances in VPC A and VPC B.
- **Peering VPC A with VPC C**: Facilitates communication between instances in VPC A and VPC C.

### Steps
1. **Create Key Pair**: Generate key pairs for the regions you intend to use, which will be needed to connect to EC2 instances later.
2. **CloudFormation Templates**: Utilize CloudFormation to create separate stacks for each VPC (A, B, and C).
3. **VPC Peering Connection Setup**:
    - Establish peering connections between VPC A and VPC B.
    - Update route tables in both VPCs to include routes for the peered VPC.
4. **Testing**:
    - Verify connectivity by SSH into an EC2 instance in VPC A using PuTTY.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/c5f8f3f2-822d-4079-8611-1ab07b362367)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/90ec56b4-a08a-4109-a2ef-86cd4525bc9a)

5. **Peer VPC A with VPC C**:
    - Set up peering connection between VPC A and VPC C.
    - Update route tables in both VPCs to enable communication between them.
6. **Testing Connectivity**:
    - Ping an EC2 instance in VPC A from an EC2 instance in VPC C to confirm successful communication.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/1a81d3be-f302-44c0-bccf-855d9480f8e6)

7. **Resource Cleanup**: After testing, clean up resources.

## Repository Structure

The repository is structured as follows:

- **`/VPC_Peering`:** Contains the reference diagram illustrating the application architecture.
