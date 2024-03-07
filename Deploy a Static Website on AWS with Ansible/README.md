# Deploy a Static Website on AWS with Ansible

This project aims to deploy a static website on AWS using Ansible for infrastructure automation. Below are the resources used and steps followed to complete this project.

## Resources Used

- VPC with public and private subnets
- NAT Gateways
- Security groups
- EC2
- Application Load Balancer
- Certificate Manager
- Route 53

## DevOps Tools

- GitHub
- Git
- Visual Studio Code
- PowerShell
- Ansible

## Ansible

Ansible is an infrastructure automation tool primarily focused on automating the deployment, configuration, and management of software applications and infrastructure. It uses an agentless architecture and relies on SSH and PowerShell remoting to communicate with target hosts. Ansible playbooks are written in YAML and describe the desired state of the system.

## Steps

1. **Build a 3-Tier AWS network VPC**
   - Enable DNS hostnames

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/a1602683-afb1-43b0-b1a7-5fb33f74439a)

   - Create an Internet Gateway

 ![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/8854a4e6-4d8a-4027-9315-9477e78649fc)

   - Create public and private subnets
 
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d91931df-3e9b-4985-9cca-9b3c5a828796)
   
   - Enable auto-assign IP settings

  ![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/f15257ec-b685-485c-9609-9ca716761f88)

   - Create route tables and associate subnets

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/67692431-4d8b-4ed6-b225-3b6e7238b922)

   - Add a public route to the Public Route Table, allowing traffic to be routed to the internet

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/fd3af5e1-3b78-4444-836c-fb194f5ad556)
 
   - Associate public subnets to the Public Route Table

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/ed12e9ea-afff-4624-bcf7-f8b2521be8ae)
   
   - Create private subnets

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/df601821-9e86-4763-8979-7c37d4f8269c)

   - Create 2 Private Route Table, NAT Gateways in each public subnet and associate each route table with the correct private subnets and add a route to the NAT Gateway for each route table

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/cdabede7-7c8d-4a83-9a95-d579866726bd)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/c490a216-2dfd-49bc-aaec-6067096d86c5)

Architecture:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/0740b81a-cc2a-49ec-8282-837a3e0f91b4)

2. **Create Security Groups**
   - For Application Load Balancer: Port: 80 and 443, Source: 0.0.0.0/0
   - For Bastion Host: Port: 22, Source: Your IP address
   - For Ansible Server: Port: 22, Source: Bastion Host Security Group
   - For Web Servers: Port: 80 and 443, Source: ALB security group; Port: 22, Source: Ansible Server security group

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/b3fe460e-57a4-451d-a524-ad4f534c938d)

3. **Launch Bastion Host and Ansible Server**
   - Install Ansible on the server

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/dec7f734-78e9-40d4-8be5-8686c9b40c6b)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/8997648b-0b6d-4603-8ea7-5d9d441110c3)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/8c9a8f4c-542d-492f-b591-bc0833aaffb0)

4. **Launch Web Servers to host the site**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/f0c934b4-6c62-47c9-8370-c1ecf8d3ba62)

Architecture:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/00c9247b-03e7-441b-97df-dcb615bbd993)

5. **Testing Connection between Ansible Server and Web Servers**

To access the web server, we are going to create a key pair on Windows, store the private key in the Ansible Server and SSH into the web servers, but firtst we need to launch new web servers using the importing key we created and SSH into those servers.

Create key pair.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/5a40241d-0516-4ad4-8ae8-9639a602f780)

Add the private key in the Ansible server.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/6352c210-6f02-4257-a7f5-9e21517e4070)

Connect to each web server. As an example:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/1b11440c-3de1-4f12-830a-5dabe1592225)

6. **Create a GitHub repository to store Ansible files**
   - Clone the repository on Ansible Server and local computer

First, verify if git is installed in the Ansible Server.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/37f13a09-11d6-41ee-860d-3822e6ff6380)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d48717dc-1c5a-4838-a4e6-c12edc99492e)

Clone the repository into your local system to create ansible file locally and push the changes to the remote repository.

Architecture:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/59019905-9f80-4ad9-9cda-5ad3c5bd14f6)

7. **Install Ansible on Ansible Server**
   - Run the following command:
```bash
# Updates packages 
sudo apt update
# Upgrades all installed packages to their latest versions
sudo apt upgrade
# Install the package which provides and abstraction of the used apt repositories and signing keys management
sudo apt install software-properties-common -y
sudo apt install ansible -y
ansible --version
```

8. **Create Ansible Inventory file**
   - Go [here](https://github.com/DDMateus/Ansible/blob/83a4ecb148ae5222e9136fe07c7398a054a94760/inventory) to see the inventory file
   - Push changes to GitHub
   - Pull changes to Ansible Server

9. **Create Ansible Configuration file**
   - Go [here](https://github.com/DDMateus/Ansible/blob/83a4ecb148ae5222e9136fe07c7398a054a94760/ansible.cfg) to see the file
   - Push changes to GitHub

10. **Pull changes into Ansible Server**

11. **Use Ansible to ping the Web Servers to test connection**
   - Run the following command:
     
```bash
ansible all -m ping
```
12. **Create Ansible Playbook to install WordPress website**
   - Go [here](https://github.com/DDMateus/Ansible/blob/83a4ecb148ae5222e9136fe07c7398a054a94760/deploywebsite.yaml) to see the file

13. **Push changes to GitHub**

14. **Pull changes into Ansible Server**

15. **Run the Playbook to install the website on Web Servers**
   - Run the following command:
     
```bash
ansible-playbook <file name>
```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/745bb977-c11d-4fe9-9dfd-413a9c1f321c)

16. **Create an Application Load Balancer**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d3046a97-675b-4dc5-a171-b84038ccf645)

Use DNS name of the application load balancer to see the website.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/626a9d0c-298e-4d25-b2e2-9e8e150937dc)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/b8a1a208-ec52-4e23-a4d6-4d17eb346e03)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/449ceace-5c1b-4466-b516-02e04899ff20)

17. **Create a Record Set in Route 53**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/424734d3-e5e5-46fa-b349-0dd814d311e6)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/e2c22ab4-b3e7-4f36-a58e-baaff527c783)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/3f6f8342-759b-46b1-bc06-c6e4d94ab5a5)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/3c79e914-a4a6-448a-8a74-9d42fbecda94)

18. **Register an SSL certificate in AWS Certificate Manager**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/51941ec6-3457-47a6-9e56-3d3eed381b42)

19. **Create an HTTPS (SSL) listener for Application Load Balancer**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/3637359b-12c8-4c36-89fb-040e7544ab9a)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/7e1b4d22-a96c-4c8c-9aa7-e3eba0b9f8f6)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/960949af-0dd1-4a02-b89d-b60a109dc8bc)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/a73368fc-6550-4ffd-8746-c5e384cb1b81)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/b20a641e-1106-49a6-b405-313b2442010a)

20. **Create an AMI for the Ansible server**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/161d392b-121c-495a-b977-789dd84cf0bb)

21. **Clean up resources**
