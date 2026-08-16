# Nginx Setup

This document explains the installation, configuration, testing, and troubleshooting of Nginx on the Ubuntu EC2 instance used in the Secure AWS Web Server project.

## 1. Update System

sudo apt update

## 2. Install Nginx

sudo apt install nginx -y

## 3. Check Nginx Status

sudo systemctl status nginx

Expected status:

Active: active (running)

## 4. Start Nginx

sudo systemctl start nginx

## 5. Enable Nginx at Boot

sudo systemctl enable nginx

This ensures that Nginx starts automatically after the server reboots.

## 6. Restart Nginx

sudo systemctl restart nginx

## 7. Reload Nginx

sudo systemctl reload nginx

Reloading applies configuration changes without completely stopping the Nginx service.

## 8. Test Nginx Configuration

Always test the configuration before reloading Nginx:

sudo nginx -t

Expected output:

syntax is ok
test is successful

## 9. Test Nginx Locally

curl localhost

Check HTTP response headers:

curl -I localhost

A successful response should contain:

HTTP/1.1 200 OK

## 10. Check Listening Ports

ss -tulpn

Nginx should normally listen on TCP port 80 for HTTP traffic.

## 11. Check IP Address

ip addr

This displays the network interfaces and IP addresses assigned to the EC2 instance.

## 12. Check Routing Table

ip route

This displays the server's routing table and helps verify network connectivity.

## 13. Access Nginx from Browser

Find the EC2 instance's Public IPv4 address from the AWS EC2 console.

Open:

http://<EC2-PUBLIC-IP>

Example:

http://13.234.XXX.XXX

If Nginx is working correctly and the security group allows HTTP traffic on port 80, the Nginx welcome page should appear.

## 14. Nginx Configuration Files

Main Nginx configuration:

/etc/nginx/nginx.conf

Available site configurations:

/etc/nginx/sites-available/

Enabled site configurations:

/etc/nginx/sites-enabled/

Default website configuration:

/etc/nginx/sites-available/default

## 15. Nginx Access Log

The access log records HTTP requests received by Nginx.

Log file:

/var/log/nginx/access.log

View the last 50 entries:

sudo tail -50 /var/log/nginx/access.log

Monitor the access log in real time:

sudo tail -f /var/log/nginx/access.log

## 16. Nginx Error Log

The error log contains Nginx errors and warnings.

Log file:

/var/log/nginx/error.log

View the last 50 entries:

sudo tail -50 /var/log/nginx/error.log

Monitor the error log in real time:

sudo tail -f /var/log/nginx/error.log

## 17. Nginx Service Logs

View Nginx logs managed by systemd:

sudo journalctl -u nginx

Show the last 50 entries:

sudo journalctl -u nginx -n 50

Monitor Nginx logs in real time:

sudo journalctl -u nginx -f

## 18. Troubleshooting

If the website is not accessible, check the following:

### Check EC2 Instance

Make sure the EC2 instance is running.

### Check Security Group

HTTP port 80 should be allowed.

Type: HTTP
Protocol: TCP
Port: 80
Source: 0.0.0.0/0

For SSH, port 22 should preferably be restricted to your IP address.

### Check Nginx Status

sudo systemctl status nginx

### Test Nginx Configuration

sudo nginx -t

### Check Listening Ports

ss -tulpn

### Test Nginx Locally

curl localhost

### Check Error Logs

sudo tail -50 /var/log/nginx/error.log

### Check Nginx Service Logs

sudo journalctl -u nginx -n 50

### Check IP Address

ip addr

### Check Routing

ip route

## 19. Nginx Troubleshooting Flow

Website not accessible
        |
        v
Check EC2 Instance
        |
        v
Check Security Group
        |
        v
Check Port 80
        |
        v
Check Nginx Status
        |
        v
Check Nginx Configuration
        |
        v
Check Listening Ports
        |
        v
Test with curl localhost
        |
        v
Check Nginx Error Logs

## 20. Important Commands

# Update package repository
sudo apt update

# Install Nginx
sudo apt install nginx -y

# Check Nginx status
sudo systemctl status nginx

# Start Nginx
sudo systemctl start nginx

# Restart Nginx
sudo systemctl restart nginx

# Reload Nginx
sudo systemctl reload nginx

# Enable Nginx at boot
sudo systemctl enable nginx

# Test Nginx configuration
sudo nginx -t

# Test Nginx locally
curl localhost

# Check HTTP headers
curl -I localhost

# Check listening ports
ss -tulpn

# Check IP address
ip addr

# Check routing table
ip route

# Check Nginx service logs
sudo journalctl -u nginx

# Show last 50 Nginx logs
sudo journalctl -u nginx -n 50

# Monitor Nginx logs
sudo journalctl -u nginx -f

# View access logs
sudo tail -50 /var/log/nginx/access.log

# View error logs
sudo tail -50 /var/log/nginx/error.log

# Monitor access logs
sudo tail -f /var/log/nginx/access.log

# Monitor error logs
sudo tail -f /var/log/nginx/error.log

## 21. Result

Nginx was successfully installed and configured on the Ubuntu EC2 instance.

The web server listens on port 80 and can be accessed using the EC2 instance's public IPv4 address.

The Nginx service, configuration, network connectivity, listening ports, and logs were verified using Linux commands.
