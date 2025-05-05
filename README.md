# Single_Endpoint_EC2-S3
A single endpoint architecture using NGINX to serve content from an S3 bucket via an EC2 instance, providing a reverse proxy for static website hosting.

# 🚀 Single Endpoint Architecture for EC2 and S3 Service

This project demonstrates how to serve a static website hosted on Amazon S3 through an Amazon EC2 instance using **Nginx as a reverse proxy**. It allows all user traffic to go through a **single endpoint** — your EC2 public IP.

---

##  Architecture overview 
### Client → EC2 Public IP → NGINX Reverse Proxy → S3 Static Website

---
## 📁 Files

- `index.html`: The HTML file hosted on the S3 bucket
- `nginx.conf`: The configuration file with reverse proxy settings
- `mybucket` (NGINX site config): The server block for routing traffic

---

## 📌 Steps to Reproduce

### 1. Create and Configure the S3 Bucket

- Name: `financebase55`
- Enable static website hosting
- Upload `index.html` with contents:
```html
<!DOCTYPE html>
<html>
<head>
    <title>My S3 Website</title>
</head>
<body>
    <h1>Welcome to Amazon S3</h1>
</body>
</html>
---

## 1. Launch an EC2 Instance
- Use Ubuntu 22.04 or Amazon Linux
- Allow ports 22 (SSH) and 80 (HTTP) in the Security Group
- Assign a static Elastic IP and associate it with the instance

## 3. Install Nginx 
sudo apt update
sudo apt install nginx -y

## 4. Configure Reverse Proxy 
Create a site config file vim /etc/nginx/sites-available/mybucket

### Paste the configuration 
server {
    listen 80;
    server_name YOUR_STATIC_PUBLIC_IP;

    location / {
        proxy_pass http://financebase55.s3-website-us-east-1.amazonaws.com/;
        proxy_set_header Host financebase55.s3-website-us-east-1.amazonaws.com;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

### Enable the site and reload NGINX

sudo ln -s /etc/nginx/sites-available/mybucket /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
 
### Test the Setup locally and remotely *(This should display the content hosted on the S bucket)
curl http://localhost
curl http://<EC2_PUBLIC_IP>
http://<your-elastic-ip>


## Architecture Diagram 
  [Client Browser]
         |
         v
[EC2 Public IP:80 (Nginx)]
         |
         v
[S3 Static Website Hosting URL]





