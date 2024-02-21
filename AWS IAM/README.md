# IAM Architecture Project

## Overview

This project focuses on Identity and Access Management (IAM) on Amazon Web Services (AWS). IAM allows you to manage access to AWS services and resources securely. This README provides instructions on launching EC2 instances with tags, creating IAM policies, testing resource access, and cleaning up resources.

## Project Steps

1. **Launch EC2 Instances with Tags:**
   - Launch two Amazon Linux 2 instances, one for development environment and one for production environments. Distinguish these instances using tags.

2. **Create AWS IAM Identities:**
   - Create an IAM policy named "EC2_Tag_Policy" using JSON method to allow all actions for EC2 instances tagged as "Env-dev" and describe-related actions for all EC2 instances. Deny create and delete tags actions to prevent users from modifying tags arbitrarily. Create an IAM User Group named "dev-group" and attach the "EC2_Tag_Policy" policy. Create an IAM User named "dev-user" and add the user to the "dev-group", allowing access to the AWS Management Console.

3. **Test Access for Resources:**
   - Log in with the "dev-user" in the AWS console and attempt to terminate the production and dev instances. Observe the error for the production instance and the stopping of the dev instance. Utilize the IAM policy simulator to test "dev-group's" permissions by simulating "DeleteTags" and "StopInstances" actions.

4. **Assign IAM Role for EC2 Instance and Test Access:**
   - Create two S3 buckets, upload an image to one of them. Create an IAM role and policy named "EC2_Bucket_Policy" for an EC2 instance, allowing access to the bucket and image object. Test the access by connecting to the instance via the EC2 instance Connect option.

5. **Clean Up Resources:**
   - Terminate development and production EC2 instances, delete IAM group, IAM user, IAM role, policies, and S3 buckets.

## Repository Structure

The repository contains the following components:

- **`/IAM_Architecture`:**
  Contains the reference diagram illustrating the IAM architecture.

- **`/EC2_Bucket_Policy.json` & `EC2_Tag_Policy.json`:**
  Contains the IAM policies used in the project.

