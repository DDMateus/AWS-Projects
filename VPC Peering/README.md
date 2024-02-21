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

![image](https://github.com/DDMateus/VPC-Peering/assets/88774178/b09eda46-1c78-4e6d-8db8-03b0e9dfe1d8)

![image](https://github.com/DDMateus/VPC-Peering/assets/88774178/ec542c25-07f4-408b-923e-9a73f13e7c83)

5. **Peer VPC A with VPC C**:
    - Set up peering connection between VPC A and VPC C.
    - Update route tables in both VPCs to enable communication between them.
6. **Testing Connectivity**:
    - Ping an EC2 instance in VPC A from an EC2 instance in VPC C to confirm successful communication.

![image](https://github.com/DDMateus/VPC-Peering/assets/88774178/56e98caf-e9d4-4c69-a084-4eee23606305)

7. **Resource Cleanup**: After testing, clean up resources.

## Repository Structure

The repository is structured as follows:

- **`/VPC_Peering`:** Contains the reference diagram illustrating the application architecture.
