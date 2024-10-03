# Terraform AWS EC2 and VPC Automation

This repository contains a series of Terraform exercises focused on provisioning AWS EC2 instances and networking infrastructure using VPC, subnets, security groups, and remote state management. These exercises demonstrate how to automate AWS infrastructure deployment with best practices like version-controlled infrastructure-as-code and remote state management.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Project Structure](#project-structure)
- [Exercises Overview](#exercises-overview)
  - [Exercise 1: Basic EC2 Provisioning](#exercise-1-basic-ec2-provisioning)
  - [Exercise 2: Parameterized EC2 Deployment](#exercise-2-parameterized-ec2-deployment)
  - [Exercise 3: EC2 with Key Pair and Provisioning](#exercise-3-ec2-with-key-pair-and-provisioning)
  - [Exercise 4: EC2 with Provisioners and Outputs](#exercise-4-ec2-with-provisioners-and-outputs)
  - [Exercise 5: EC2 with Remote Backend](#exercise-5-ec2-with-remote-backend)
  - [Exercise 6: EC2 and VPC with Subnets, EBS Volumes](#exercise-6-ec2-and-vpc-with-subnets-ebs-volumes)
- [Web Application Setup](#web-application-setup)
- [How to Run](#how-to-run)

## Prerequisites
- [Terraform](https://www.terraform.io/downloads.html) >= 1.0.0
- AWS account with appropriate permissions
- SSH key pair (public and private)
- S3 bucket for remote state management (for exercises 5 and 6)
- IAM permissions for managing EC2, VPC, and S3 resources

## Project Structure
```
├── exercise_1/
│   ├── first_instance.tf
├── exercise_2/
│   ├── instance.tf
│   ├── providers.tf
│   ├── vars.tf
├── exercise_3/
│   ├── instance.tf
│   ├── providers.tf
│   ├── vars.tf
│   ├── web.sh
├── exercise_4/
│   ├── instance.tf
│   ├── providers.tf
│   ├── vars.tf
│   ├── web.sh
├── exercise_5/
│   ├── backend.tf
│   ├── instance.tf
│   ├── providers.tf
│   ├── vars.tf
│   ├── web.sh
├── exercise_6/
│   ├── backend.tf
│   ├── instance.tf
│   ├── secgrp.tf
│   ├── vars.tf
│   ├── vpc.tf
│   ├── web.sh
└── README.md
```

## Exercises Overview

### Exercise 1: Basic EC2 Provisioning
In this exercise, we create a basic EC2 instance using an AMI and a key pair. It demonstrates simple EC2 provisioning with security group association and tagging.

**File:** `exercise_1/first_instance.tf`

### Exercise 2: Parameterized EC2 Deployment
This exercise introduces the use of variables and mapping for different regions and availability zones, making the Terraform configuration more reusable across multiple regions.

**Files:**
- `exercise_2/instance.tf`
- `exercise_2/providers.tf`
- `exercise_2/vars.tf`

### Exercise 3: EC2 with Key Pair and Provisioning
In this exercise, a new EC2 instance is provisioned with a key pair resource and provisioning scripts to install a web server. A file provisioner and remote-exec provisioner are used to deploy a simple web page on the instance.

**Files:**
- `exercise_3/instance.tf`
- `exercise_3/web.sh`

### Exercise 4: EC2 with Provisioners and Outputs
Building on Exercise 3, this task adds Terraform outputs to return the public and private IP addresses of the EC2 instance. The web server is automatically deployed using a script.

**Files:**
- `exercise_4/instance.tf`
- `exercise_4/web.sh`

### Exercise 5: EC2 with Remote Backend
This exercise introduces remote state management using an S3 bucket. The EC2 instance is provisioned similarly to the previous exercises, but the Terraform state is stored in an S3 backend.

**Files:**
- `exercise_5/backend.tf`
- `exercise_5/instance.tf`
- `exercise_5/web.sh`

### Exercise 6: EC2 and VPC with Subnets, EBS Volumes
The final exercise involves creating a custom VPC with public and private subnets across three availability zones. It provisions EC2 instances in a public subnet, attaches an EBS volume, and sets up security groups.

**Files:**
- `exercise_6/backend.tf`
- `exercise_6/instance.tf`
- `exercise_6/secgrp.tf`
- `exercise_6/vpc.tf`
- `exercise_6/web.sh`

## Web Application Setup
The EC2 instances are configured to serve a sample HTML template from [Tooplate](https://www.tooplate.com). The `web.sh` script provisions an Apache HTTP server and downloads the website template, which is then hosted on the instance.

**Steps in `web.sh`:**
- Install `httpd` (Apache) on the EC2 instance
- Start and enable the `httpd` service
- Download and unzip the website template
- Copy the website files to `/var/www/html/`
- Restart the web server

## How to Run

1. **Clone the repository:**
   ```bash
   git clone https://github.com/arakashnawin/terraform-exercises.git
   cd terraform-exercises
   ```

2. **Configure AWS CLI:**
   Ensure you have configured AWS credentials on your local machine using `aws configure`.

3. **Set up your SSH keys:**
   Ensure your SSH key pair is available (`dovekey` and `dovekey.pub`) and place them in the repository folder.

4. **Initialize Terraform:**
   ```bash
   terraform init
   ```

5. **Apply the Terraform configuration:**
   ```bash
   terraform apply
   ```
   Review the plan and type `yes` to deploy the resources.

6. **Access the EC2 instance:**
   After the Terraform run is complete, you can SSH into the EC2 instance using:
   ```bash
   ssh -i dovekey ec2-user@<Public-IP>
   ```
   You can also visit the public IP in your browser to view the hosted web page.
