# Troubleshooting Guide

This document contains common troubleshooting steps for the Secure AWS Web Server project.

## 1. EC2 Instance Is Not Accessible

First, verify that the EC2 instance is running.

Check in:

AWS Console → EC2 → Instances

The instance state should be:

Running

Also verify that the instance has a Public IPv4 address.

---

## 2. Cannot Connect to EC2 Using SSH

SSH command:

ssh -i <key-file.pem> ubuntu@<EC2-PUBLIC-IP>

Check the following:

- EC2 instance is running
- Public IPv4 address is correct
- Correct `.pem` key is being used
- Key file permissions are correct
- Security Group allows TCP port 22
- SSH source is allowed from your current IP address
- Route table has a route to the Internet Gateway
- Internet Gateway is attached to the VPC

Fix key permissions if required:

chmod 400 <key-file.pem>

Example:

ssh -i secure-web-key.pem ubuntu@<EC2-PUBLIC-IP>

---

## 3. Website Is Not Opening

If this URL does not work:

http://<EC2-PUBLIC-IP>

Check the following in order:

1. EC2 instance is running
2. EC2 has a public IPv4 address
3. Security Group allows HTTP port 80
4. Public subnet is associated with the correct route table
5. Route table contains `0.0.0.0/0`
6. Internet Gateway is attached to the VPC
7. Nginx is running
8. Nginx is listening on port 80

---

## 4. Check Nginx Status

Run:

sudo systemctl status nginx

If Nginx is stopped:

sudo systemctl start nginx

If Nginx fails to start:

sudo nginx -t

Then check the logs:

sudo journalctl -u nginx -n 50

---

## 5. Nginx Configuration Error

Before restarting or reloading Nginx, test the configuration:

sudo nginx -t

If the configuration is valid:

syntax is ok
test is successful

If there is an error, Nginx will show the configuration file and line number where the problem exists.

After fixing the configuration:

sudo nginx -t
sudo systemctl reload nginx

---

## 6. Check Whether Nginx Is Listening on Port 80

Run:

sudo ss -tulpn | grep :80

Expected result should show Nginx listening on TCP port 80.

You can also use:

ss -tulpn

If Nginx is not listening on port 80, check:

sudo systemctl status nginx
sudo nginx -t

---

## 7. Test Nginx Locally

Run the following command from the EC2 instance:

curl localhost

If Nginx is working, you should receive an HTML response.

Also test the HTTP headers:

curl -I localhost

Expected response:

HTTP/1.1 200 OK

If `curl localhost` works but the website is not accessible from your browser, the problem is likely related to AWS networking or the Security Group.

---

## 8. Check Security Group

The Security Group should contain an HTTP rule:

Type: HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0

For SSH:

Type: SSH
Protocol: TCP
Port: 22
Source: My IP

Avoid allowing SSH from `0.0.0.0/0` unless it is specifically required for testing.

---

## 9. Check Route Table

The public route table should contain:

Destination: 10.0.0.0/16
Target: local

Destination: 0.0.0.0/0
Target: secure-web-igw

The route table must also be associated with:

secure-web-public-subnet

---

## 10. Check Internet Gateway

Verify that the Internet Gateway:

secure-web-igw

is attached to:

secure-web-vpc

If the Internet Gateway is not attached to the VPC, the public subnet cannot communicate with the internet through it.

---

## 11. Check Subnet

Verify the public subnet configuration:

Subnet Name: secure-web-public-subnet
CIDR: 10.0.1.0/24
VPC: secure-web-vpc

Make sure the subnet is associated with:

secure-web-public-rt

Also verify that Auto-assign Public IPv4 is enabled if the EC2 instance is expected to receive a public IPv4 address automatically.

---

## 12. Check IP Address

Run:

ip addr

This shows the private IP address assigned to the EC2 instance.

To check the routing table:

ip route

The default route should point toward the appropriate network gateway.

---

## 13. Check Internet Connectivity

Test connectivity to the internet:

ping -c 4 8.8.8.8

Test DNS resolution:

ping -c 4 google.com

If the IP address works but the domain name does not work, investigate DNS configuration.

---

## 14. Check Nginx Access Logs

View recent requests:

sudo tail -50 /var/log/nginx/access.log

Monitor requests in real time:

sudo tail -f /var/log/nginx/access.log

If you open the website in a browser while monitoring the access log, you should see the request appear.

---

## 15. Check Nginx Error Logs

View recent errors:

sudo tail -50 /var/log/nginx/error.log

Monitor errors in real time:

sudo tail -f /var/log/nginx/error.log

The error log is useful for diagnosing:

- Configuration errors
- Permission problems
- Port conflicts
- Missing files
- Upstream connection problems
- Server errors

---

## 16. Check Systemd Nginx Logs

View all Nginx service logs:

sudo journalctl -u nginx

View the last 50 entries:

sudo journalctl -u nginx -n 50

Monitor logs in real time:

sudo journalctl -u nginx -f

Show only error-level messages:

sudo journalctl -u nginx -p err

---

## 17. Port 80 Is Already in Use

Check which process is using port 80:

sudo ss -tulpn | grep :80

If another service is using port 80, Nginx may fail to start.

Identify the process and determine whether it should be stopped or configured to use another port.

---

## 18. Nginx Service Failed After Configuration Change

If Nginx was working before a configuration change and then stopped working:

sudo nginx -t

If the configuration test succeeds:

sudo systemctl restart nginx

If it fails:

sudo journalctl -u nginx -n 50

Then check:

sudo tail -50 /var/log/nginx/error.log

Fix the configuration error and test again:

sudo nginx -t

---

## 19. Disk Space Problem

Check disk usage:

df -h

Check available block devices:

lsblk

If the root filesystem is almost full, investigate large files and logs.

Check the size of Nginx logs:

sudo du -sh /var/log/nginx/*

Large log files can cause services to behave unexpectedly when the filesystem becomes full.

---

## 20. Nginx Process Check

Check running Nginx processes:

ps aux | grep nginx

You can also check the service:

sudo systemctl status nginx

---

## 21. Common HTTP Errors

### HTTP 403 Forbidden

Possible causes:

- Incorrect file permissions
- Incorrect Nginx configuration
- Directory permissions
- Access restrictions

Check:

sudo tail -50 /var/log/nginx/error.log

### HTTP 404 Not Found

Possible causes:

- Requested file does not exist
- Incorrect document root
- Incorrect URL

Check the Nginx configuration and website files.

### HTTP 500 Internal Server Error

Possible causes:

- Application error
- Incorrect server configuration
- Permission problem
- Backend application failure

Check:

sudo tail -50 /var/log/nginx/error.log

### Connection Refused

Possible causes:

- Nginx is not running
- Port 80 is not listening
- Security Group is blocking traffic
- Incorrect network configuration

Check:

sudo systemctl status nginx
ss -tulpn
curl localhost

---

## 22. Complete Troubleshooting Workflow

When the website is not working, follow this order:

1. Check EC2 instance status
2. Check Public IPv4 address
3. Check Security Group
4. Check Public Subnet
5. Check Route Table
6. Check Internet Gateway
7. Check Nginx status
8. Test Nginx configuration
9. Check listening ports
10. Test using `curl localhost`
11. Check Nginx access logs
12. Check Nginx error logs
13. Check systemd logs
14. Check disk space
15. Test internet connectivity

---

## 23. Quick Troubleshooting Commands

sudo systemctl status nginx

sudo nginx -t

sudo systemctl restart nginx

sudo systemctl reload nginx

ss -tulpn

sudo ss -tulpn | grep :80

curl localhost

curl -I localhost

ip addr

ip route

df -h

lsblk

sudo journalctl -u nginx -n 50

sudo journalctl -u nginx -f

sudo tail -50 /var/log/nginx/access.log

sudo tail -50 /var/log/nginx/error.log

ping -c 4 8.8.8.8

---

## 24. Troubleshooting Flow

EC2 Web Server Not Accessible
        |
        v
Is EC2 Running?
        |
        v
Does EC2 Have Public IPv4?
        |
        v
Check Security Group
        |
        v
Is Port 80 Allowed?
        |
        v
Check Public Subnet
        |
        v
Check Route Table
        |
        v
Is 0.0.0.0/0 → Internet Gateway?
        |
        v
Check Internet Gateway
        |
        v
Check Nginx Status
        |
        v
sudo nginx -t
        |
        v
Check Port 80
        |
        v
curl localhost
        |
        v
Check Nginx Error Logs
        |
        v
Fix the Problem
        |
        v
Test Website Again

---

## 25. Final Verification

After troubleshooting, verify the complete web server:

sudo systemctl status nginx

sudo nginx -t

ss -tulpn | grep :80

curl -I localhost

ip addr

ip route

df -h

sudo journalctl -u nginx -n 50

The website should then be accessible using:

http://<EC2-PUBLIC-IP>

## Result

The troubleshooting process verifies the AWS network configuration, EC2 instance, Security Group, routing, Internet Gateway, Nginx service, ports, logs, and disk space.

This provides a systematic approach to identifying and resolving common problems in the Secure AWS Web Server project.
