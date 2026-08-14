# secure-aws-web-server
Secure AWS web server deployment using EC2, VPC, IAM, EBS, S3, Security Groups, and Nginx.


# Secure AWS Web Server

## Project Overview

This project demonstrates the deployment of a web server on AWS using Ubuntu Server, Amazon EC2, VPC, Security Groups, IAM, EBS, S3, and Nginx.

The project was built to develop practical AWS Cloud Support skills including cloud networking, Linux server administration, web server configuration, security, storage, and troubleshooting.

---

## Objectives

- Create a custom AWS VPC
- Configure a public subnet
- Configure an Internet Gateway
- Configure a route table
- Configure Security Groups
- Launch an Ubuntu EC2 instance
- Connect to the server using SSH
- Use EBS for EC2 storage
- Configure IAM permissions
- Use S3 for storage/backup
- Install and configure Nginx
- Deploy a website from GitHub
- Troubleshoot common server and connectivity issues

---

## Architecture

![AWS Architecture](architecture.png)

### Architecture Flow

```text
Internet
   |
Internet Gateway
   |
VPC
   |
Public Subnet
   |
Security Group
   |
EC2 - Ubuntu Server
   |
Nginx
   |
Website
   |
S3 - Backup/Storage