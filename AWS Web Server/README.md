# AWS Web Server Deployment Project

## Overview

This project demonstrates the deployment of a simple web server on Amazon Web Services (AWS) using an EC2 Linux instance.

## Project Steps

1. **Create a New Key Pair:**
   - Create an SSH key pair to manage EC2 instances. This key pair will be used to access the EC2 instance.

2. **Launch a Web Server Instance:**
   - Launch an Amazon Linux 2 instance, bootstrap Apache/PHP, and install a basic web page that will display information about our instance.

3. **Connect to EC2 Instance using PuTTY (Optional):**
   - Connect to the EC2 instance using an SSH client like PuTTY. This step is optional and may be required for Windows users.

4. **Clean Up Resources:**
   - Delete the EC2 instance to avoid unnecessary charges and clean up resources.

## Scripts

The repository contains the following script for deploying the web app on the EC2 instance:

```bash
#!/bin/sh

# Install a LAMP stack
dnf install -y httpd wget php-fpm php-mysqli php-json php php-devel
dnf install -y mariadb105-server
dnf install -y httpd php-mbstring

# Start the web server
chkconfig httpd on
systemctl start httpd

# Install the web pages for the lab
if [ ! -f /var/www/html/immersion-day-app-php7.zip ]; then
   cd /var/www/html
   wget -O 'immersion-day-app-php7.zip' 'https://static.us-east-1.prod.workshops.aws/public/29b35877-4a64-4c0d-8579-a43a5f2187ca/assets/immersion-day-app-php7.zip'
   unzip immersion-day-app-php7.zip
fi

# Install the AWS SDK for PHP
if [ ! -f /var/www/html/aws.zip ]; then
   cd /var/www/html
   mkdir vendor
   cd vendor
   wget https://docs.aws.amazon.com/aws-sdk-php/v3/download/aws.zip
   unzip aws.zip
fi

# Update existing packages
dnf update -y
```
## Repository Structure

The repository is structured as follows:

- **`/Application_Architecture`:**
   Contains the reference diagram illustrating the application architecture.

- **`/Deployment_Scripts`:**
   Contains the scripts used to deploy the web server on the EC2 instance.

