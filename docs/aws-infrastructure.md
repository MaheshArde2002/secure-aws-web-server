# AWS Infrastructure — Secure Web Server

This document describes the AWS infrastructure created for the Secure Web Server project.

## Architecture

The infrastructure contains:

- 1 VPC
- 1 Public Subnet
- 1 Internet Gateway
- 1 Route Table
- 1 Security Group
- 1 EC2 Instance
- Nginx Web Server

---

## 1. VPC

A custom VPC was created to provide an isolated network environment for the web server.

### Configuration

- **VPC Name:** `secure-web-vpc`
- **CIDR Block:** `10.0.0.0/16`

The VPC provides the main network environment for the web server infrastructure.

---

## 2. Public Subnet

One public subnet was created inside the VPC for the EC2 web server.

### Configuration

- Subnet Name: public-subnet
- VPC: `secure-web-vpc`
- CIDR Block: `10.0.1.0/24`
- Type: Public Subnet
- Auto-assign Public IPv4: Enabled

The subnet is associated with the public route table.

---

## 3. Internet Gateway

An Internet Gateway was created and attached to the VPC.

### Configuration

- Internet Gateway Name: `secure-web-igw`
- Attached VPC:`secure-web-vpc`

The Internet Gateway provides internet connectivity for resources in the public subnet.

---

## 4. Route Table

One public route table was created for the public subnet.

### Configuration

- Route Table Name: public-route-table
- VPC: `secure-web-vpc`

### Routes

| Destination | Target |
|---|---|
| `10.0.0.0/16` | Local |
| `0.0.0.0/0` | `secure-web-igw` |

The `0.0.0.0/0` route sends internet-bound traffic to the Internet Gateway.

The route table is associated with:

`secure-web-public-subnet`

---

## 5. Security Group

A security group was created to control network traffic to the EC2 web server.

### Configuration

- Security Group Name: `secure-web-sg`
- VPC: `secure-web-vpc`

### Inbound Rules

| Type | Protocol | Port | Source |
|---|---|---:|---|
| SSH | TCP | 22 | My IP |
| HTTP | TCP | 80 | `0.0.0.0/0` |
| HTTPS | TCP | 443 | `0.0.0.0/0` |

SSH is restricted to the administrator's IP address.

HTTP and HTTPS are allowed so users can access the web server.

---

## 6. EC2 Web Server

An EC2 instance was launched inside the public subnet.

### Configuration

- Instance Name: `secure-web-server`
- Operating System: Ubuntu Server 24.04 LTS
- VPC: `secure-web-vpc`
- Subnet: `secure-web-public-subnet`
- Security Group: `secure-web-sg`
- Public IPv4: Enabled

The EC2 instance is used as the web server.

---

## 7. Connect to EC2

Connect to the EC2 instance using SSH:

```bash
ssh -i <key-file.pem> ubuntu@<EC2-PUBLIC-IP>
