sudo apt update
sudo apt install nginx -y

sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx

sudo nginx -t

curl localhost

ip addr
ip route
ss -tulpn

lsblk
df -h

journalctl -u nginx

