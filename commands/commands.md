# Update System
sudo apt update

# Install Nginx
sudo apt install nginx -y

# Check status of Nginx
sudo systemctl status nginx

# Start Nginx Service
sudo systemctl start nginx

# Restart Nginx Service
sudo systemctl restart nginx

# Reload Nginx Service
sudo systemctl reload nginx

# Enable Nginx at boot
sudo systemctl enable nginx

# Test Nginx Configuration
sudo nginx -t

# Test Nginx using curl
curl localhost

# Check HTTP Headers
curl -I localhost

# Check IP Address
ip addr

# Check Routing Table
ip route

# Check Listening Ports
ss -tulpn

# Check Block Devices / Disks
lsblk

# Check Disk Space
df -h

# Check Nginx Logs
sudo journalctl -u nginx

# Show Last 50 Nginx Logs
sudo journalctl -u nginx -n 50

# Monitor Nginx Logs Live
sudo journalctl -u nginx -f

# Check Nginx Access Log
sudo tail -f /var/log/nginx/access.log

# Check Nginx Error Log
sudo tail -f /var/log/nginx/error.log
