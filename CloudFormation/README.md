# AWS CloudFormation

<p align="justify">AWS CloudFormation is a powerful service that simplifies the management of AWS resources by allowing you to define your infrastructure as code. With CloudFormation, you can create templates that describe all the resources you need, such as EC2 instances, RDS database instances, and more. CloudFormation then takes care of provisioning and configuring these resources for you, saving you time and effort.</p>

## Setting Up a VPC

In this section, we'll walk through creating a basic VPC (Virtual Private Cloud) using CloudFormation.

### Instructions:
1. Open your preferred text editor and create a new YAML file called `sfid-cfn-vpc.yaml`.
2. Copy and paste the sample CloudFormation template provided below:

```yaml
Resources:
  # Create a VPC
  MainVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
```

3. Save the file and navigate to the CloudFormation console.
4. Click on "Create stack" and upload the template file (`sfid-cfn-vpc.yaml`).

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/ffbf21e6-0b64-4dab-b5e9-50075d06b5fe)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/8cf0679d-8b7a-46fb-bf8c-b45137c3d555)

5. Provide a name for your stack and follow the on-screen instructions to create it.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d6ed6772-8820-4027-ab13-5a3eeecee43e)

6. Once the stack is created, you can view your new VPC in the VPC console.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/831eb939-4d98-485c-874d-6663f0cad837)

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/37d4d3dc-3a8f-4aa9-a7c6-6f0e33041951)

### Additional Configuration:
- To enable DNS hostnames and support for your VPC, add the following code to the YAML file:

```yaml
    EnableDnsHostnames: 'true'
    EnableDnsSupport: 'true'
    Tags:
      - Key: Name
        Value: VPC for CF
```

- Update the CloudFormation stack with the modified template to apply the changes.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/f8f8d57e-1f0f-40ed-a67c-955f224e76e1)

## Creating Additional Resources
<p align="justify">You can extend your CloudFormation template to create additional resources such as subnets, routing tables, security groups, internet gateway, and more. Below are examples of how to create some of these resources:</p>

```yaml
# Create a Subnet
FirstSubnet:
  Type: AWS::EC2::Subnet
  Properties:
    VpcId: !Ref MainVPC
    CidrBlock: 10.0.10.0/24
    AvailabilityZone: "us-east-2a"
    Tags:
      - Key: Name
        Value: Public Subnet A
```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/a588a2f7-17e5-446c-aa60-6d2fac8c0f01)

```yaml
# Create additional subnet
Resources:
  SecondSubnet:
    Type: AWS::EC2::Subnet
    Properties:
      VpcId: !Ref MainVPC
      CidrBlock: 10.0.20.0/24
      AvailabilityZone: "us-east-2b"
      Tags:
        - Key: Name
          Value: Public Subnet B - SFID
```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/d8248b3c-78c8-4af2-a616-961df3717f22)

```yaml
# Create and set Public Route Table
Resources:
  PublicRouteTable:
    Type: AWS::EC2::RouteTable
    Properties:
      VpcId:  !Ref MainVPC
      Tags:
        - Key: Name
          Value: Public Route Table

  PublicRoute:
    Type: 'AWS::EC2::Route'
    DependsOn: AttachIGW
    Properties:
      RouteTableId: !Ref PublicRouteTable
      DestinationCidrBlock: 0.0.0.0/0
      GatewayId: !Ref InternetGateway

  PublicSubnet1RouteTableAssociation:
    Type: 'AWS::EC2::SubnetRouteTableAssociation'
    Properties:
      SubnetId: !Ref FirstSubnet
      RouteTableId: !Ref PublicRouteTable

  PublicSubnet2RouteTableAssociation:
    Type: 'AWS::EC2::SubnetRouteTableAssociation'
    Properties:
      SubnetId: !Ref SecondSubnet
      RouteTableId: !Ref PublicRouteTable
```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/a23f50bb-a369-4619-9ee0-25047b6e08fe)

```yaml
# Create a Security Group
MainSecurityGroup:
  Type: AWS::EC2::SecurityGroup
  Properties:
    GroupDescription: Security Group for Web Server
    VpcId: !Ref MainVPC
    SecurityGroupIngress:
      - IpProtocol: tcp
        FromPort: 80
        ToPort: 80
        CidrIp: <your IP address>
    Tags:
      - Key: Name
        Value: Web Server Security Group - SFID
```

```yaml
# Create Internet Gateway
Resources:
  InternetGateway:
    Type: AWS::EC2::InternetGateway
    DependsOn: MainVPC

  AttachIGW:
    Type: AWS::EC2::VPCGatewayAttachment
    Properties:
      VpcId: !Ref MainVPC
      InternetGatewayId: !Ref InternetGateway
```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/7e79fe73-1c19-447c-9acd-33671dbdfb92)

## Outputs
<p align="justify">After deploying your CloudFormation stack, you can view outputs that provide useful information about the resources created. For example, you can retrieve the ID of the public subnet or the security group. Here's an example of how to define outputs:</p>

```yaml
Outputs:
  MainSubnet:
    Value: !Ref FirstSubnet
    Description: Public Subnet ID with Direct Internet Route

  MainSecurityGroup:
    Value: !Ref MainSecurityGroup
    Description: Security Group ID for the Web Server
```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/79d4ecdf-3bd4-4436-93fa-f1a537b6ccad)

## Setting Up an EC2 Instance

### Instructions:

```yaml
# Define AMI and Instance Type
Resources:
  # Create EC2 Linux
  WebServerInstance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: "ami-0331ebbf81138e4de"
      InstanceType: t3a.micro
```

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/8605e86d-2c91-4875-a351-a1ecaf0ce8b3)

Configure User Data to install and start Apache HTTP server and create a basic HTML page.

```yaml
# Define Tags and User Data
Tags:
  - Key: Name
    Value: Web Server for IMD
UserData: 
  Fn::Base64: |
    #!/bin/sh
    yum -y install httpd
    chkconfig httpd on
    systemctl start httpd
    echo '<html><center><text="#252F3E" style="font-family: Amazon Ember"><h1>AWS CloudFormation is Fun !!!</h1>' > /var/www/html/index.html
    echo '<h3><img src="https://d0.awsstatic.com/logos/powered-by-aws.png"></h3></html>' >> /var/www/html/index.html
```

The EC2 instance was launched to the Default VPC, using a default security group which doesn’t have access to the internet.

Launch the EC2 instance in the VPC created earlier using CloudFormation.

This parameters allows users to input subnet and security groups Ids when deploying the stack.

```yaml
Parameters:
  PublicSubnet:
    Description: Select a Public Subnet created in the "VPC for CF"
    Type: 'AWS::EC2::Subnet::Id'
  SecurityGroup:
    Description: Select the Security Group created in the "VPC for CF"
    Type: 'AWS::EC2::SecurityGroup::Id'
```

Lets define and configure a network interface for EC2 instance.

```yaml
NetworkInterfaces:
  - GroupSet:
      - !Ref SecurityGroup
    AssociatePublicIpAddress: 'true'
    DeviceIndex: '0'
    DeleteOnTermination: 'true'
    SubnetId: !Ref PublicSubnet
```

Try to acess the site.

![image](https://github.com/DDMateus/AWS-Projects/assets/88774178/9e26f9c2-fdca-4da1-ae94-ff34b93a50b7)

## Outputs

```yaml
Outputs:
  PublicDNS:
    Value: !Join 
      - ''
      - - 'http://'
        - !GetAtt 
          - WebServerInstance
          - PublicDnsName
    Description: Web Host Public URL
```
