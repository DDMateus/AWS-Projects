## Deploy a static web app on AWS with Docker

### Architecture:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/9febb910-1e27-472d-a63c-f7925729f4a2)

- **AWS Services:**
  - VPC
  - NAT Gateways
  - Security Groups
  - Application Load Balancer
  - Amazon ECR
  - Amazon ECS
  - Certificate Manager
  - Route 53

- **DevOps Tools:**
  - Docker
  - Docker Hub
  - Git
  - GitHub
  - PowerShell
  - Visual Studio Code

### Setup Instructions:

1. **Clone the repository on your computer**
   - `git clone <repository-url>`

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/88fafee8-028e-4f6a-88fa-9d87bff13b3b)

2. **Sign up for a Docker Hub account**
   - Create an account on [Docker Hub](https://hub.docker.com/) to store your Docker images.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/42b816f7-0c42-4773-88ba-0e6994f7a587)

3. **Enable virtualization on your computer**
   - Virtualization should be enabled by default. To check, run:
     ```powershell
     systeminfo.exe
     ```
     Ensure that "Virtualization Enabled In Firmware" is set to "Yes".

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/60484d64-7d02-43da-8d49-e6c4abdeb63d)

4. **Download and install Docker**
   - Download and install Docker Desktop from the [official Docker website](https://www.docker.com/products/docker-desktop).

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d43c633e-0592-440b-affe-79c2d7888718)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/cf2d2a09-2fe3-4ffb-b93a-8ea2d757f7d8)

5. **Create Dockerfile to build the container image**
   - Use Visual Studio Code to create a `Dockerfile` in the repository folder.
   - The Dockerfile is available [here](https://github.com/DDMateus/Docker/tree/3e7f89da01e4aaee04fb456508cf34dc1b25ee45/jupiter-website).

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/2484f22b-8be2-4625-9f94-ebae2f01f1f1)

6. **Build the container image**
   - Open a terminal in the repository folder and run:
     ```powershell
     docker build -t jupiter .
     ```
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/abdf3099-8757-4cd5-85bb-c61c4aa9e3db)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/dbf822d6-2947-4e5e-b2f4-eb8889b705dd)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/380fe5b2-0997-47cf-a40f-3c2c0e2cc9be)

7. **Start the container**
   - Run the following command to start a container:
     ```powershell
     docker run -d -p 81:80 jupiter
     ```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/45876d32-7932-4390-a27b-ec9f942f8e76)

Website:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/c3390938-634f-4b63-acb1-b075c3fadedd)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/2afd4d0d-e355-4439-a2bf-30a8275d7d87)

8. **Push the image to Docker Hub repository**
   - Log in to Docker Hub:
     ```powershell
     docker login -u <username>
     ```
          
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/c13a0d96-c9f3-4a25-ab9f-c67cb587e1db)

   - Tag the image:
     ```powershell
     docker tag jupiter <username>/<image-name>
     ```
     
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/df9788af-3bad-4b8b-bbce-883c83c4efb9)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/0447318a-aae0-4ce6-bbd9-ff841dc139e0)

   - Push the image to the repository:
     ```powershell
     docker push <username>/<image-name>
     ```
     
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/95e39a98-c1ec-41bd-a8ce-520550042bb6)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/242bb3cd-ab9b-4772-878a-4d7f0a3eee62)

9. **Create an IAM user and configure AWS CLI**
   - Create an IAM user with programmatic access.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/f065cc3f-a7e1-493b-8466-64212ef0160d)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/4072a51e-1dc7-46d9-9140-43ff8c8540d7)

   - Configure AWS CLI with the user's credentials:
     ```powershell
     aws configure
     ```

10. **Create an Amazon ECR repository**
    - Using AWS CLI:
      ```powershell
      aws ecr create-repository --repository-name <name> --region us-east-1
      ```
      
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/c91cf17c-58d1-4fd1-89c6-2395ae8700e0)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/9e0c0397-b587-4ccb-8f4d-4e31c59c9ca5)

11. **Push the image to your ECR Repository**
    - Authenticate Docker with Amazon ECR:
      ```powershell
      aws ecr get-login-password | docker login --username AWS --password-stdin <aws-account-id>.dkr.ecr.<region>.amazonaws.com
      ```
      
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/82a00338-2c17-4b01-9963-704d48b4b796)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/9ac82d41-2875-47fa-9c44-2d8c81cd0f54)

  - Push the image to the ECR repository:
      ```powershell
      docker push <uri-repository>
      ```
      
![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/118e949a-6268-4cc4-96b3-fa770004ee48)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/134972ff-08f2-47df-b965-6917faa5fd95)


12. **Build a 2-tier AWS network VPC**

VPC:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/b56ae5c2-0b2e-4794-b680-807b52f07cf4)

Internet Gateway:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/13c58c14-d25e-4e8b-b9f1-52d9ff1642d8)

Public Subnets:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/92ab6cf4-251e-43de-8970-21addab504a7)

Public Route Table:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/e6f8dbe0-b5c7-46bb-9ad6-93a4b801bcc9)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/67798ab6-3df6-42bc-b15f-a28560b4e6fc)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/47c33a40-7f22-43a8-a1df-800b99e11e1c)

Private Subnets:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/85abc34e-6414-413c-8758-0abc22100d7a)

NAT Gateways:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/10aba291-a29d-44cc-a5c7-7e1f7c4915d8)

Private Route Tables:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/27352b60-b0b4-4661-b166-e5f03d476452)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/ae9940f5-7f52-4b39-b902-040a2281b5cb)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/a68127b6-0e72-44b6-8bed-2637319c829f)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/fa0e8fbf-9e65-497e-9c0e-1c93d433674d)

Security groups:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/043fbf59-2f84-428b-9beb-6f5c30b0b9d9)

Target group:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/6fc14d26-604e-4d8a-b0e9-71dc8092baf7)

Application Load Balancer:

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/65af4a68-e827-42cb-9937-7f7bd1491662)

13. **Create an ECS Cluster**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/1ca0be76-abcc-422a-97a7-a58de31b90a2)

14. **Create Task Definition**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/82c55176-56fc-466c-9971-836598a08611)

15. **Create an ECS Service**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/a8c3b1e0-4a0a-4021-8a3d-095e460a0bdf)

16. **Use a new domain name with Route 53**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/e8bc6a25-ebe7-445b-a476-5623fed30ccf)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/19409d1a-7344-4d22-baf9-a008a0d7308f)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/31c81549-9a49-458e-b3e5-40fefb9997ed)

17. **Register for an SSL certificate in AWS Certificate Manager**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/dde2b939-7ede-410a-8faf-20db0c38a719)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/93d1f684-6377-4f03-af48-7c5a9ead2df2)

18. **Create an HTTPS (SSL) listener for ALB**

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/900a035a-5e42-4aff-ab9c-59feba0016c0)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/1e154b71-f667-43c3-93f4-e3d3d983f750)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/9ee3bc11-95bd-40d1-944c-7f5c0c01f074)

19. **Clean Up resources**
