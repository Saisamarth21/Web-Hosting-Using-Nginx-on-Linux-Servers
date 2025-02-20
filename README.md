# Hosting a Website Using Nginx on Linux (WSL or Virtual Machine)

This guide provides step-by-step instructions for hosting a website using Nginx on Linux, whether you're using Windows Subsystem for Linux (WSL) or a virtual machine. It covers both scenarios: hosting with a domain and hosting without a domain.

---

## **Overview**

Nginx is a high-performance web server that can be used to serve static or dynamic websites. In this guide, we will:

1. Set up Nginx on a Linux environment (WSL or VM).
2. Host a simple website.
3. Configure Nginx for hosting with or without a domain.

## Prerequisites

- **Ubuntu/WSL/Linux:** A Linux environment where you can install and run NGINX.
- **NGINX:** A web server installed on your system.
- (Optional) **Port Forwarding/Router Configuration:** If you want external access using a public IP.

## Installation and Setup

### 1. Install NGINX

Update your package lists and install NGINX:
```bash
sudo apt update
sudo apt install nginx -y
```

After installation, start and enable Nginx to run on boot:
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

Verify that Nginx is running:
```bash
sudo systemctl status nginx
```
You should see `active (running)` in the output.

### 2. Configure Firewall

If you're using a virtual machine or a cloud server, ensure the firewall allows HTTP/HTTPS traffic:
```bash
sudo ufw allow 'Nginx Full'
```

Check the status of the firewall:
```bash
sudo ufw status
```

### 3. Host a Website Without a Domain

#### Step 1: Create a Website Directory

Create a directory for your website files:
```bash
sudo mkdir -p /var/www/mywebsite
```

Set the correct permissions:
```bash
sudo chown -R $USER:$USER /var/www/mywebsite
sudo chmod -R 755 /var/www/mywebsite
```

#### Step 2: Create a Sample HTML File

Create a simple `index.html` file:
```bash
nano /var/www/mywebsite/index.html
```
Add the Your content

#### Step 3: Configure Nginx

Create a new Nginx configuration file for your website:
```bash
sudo nano /etc/nginx/sites-available/mywebsite
```

Add the following configuration:

```bash
server {
    listen 80;
    server_name _;

    root /var/www/mywebsite;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```


Enable the configuration by creating a symbolic link:
```bash
sudo ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/
```

Test the Nginx configuration for errors:
```bash
sudo nginx -t
```

If the test is successful, reload Nginx:
```bash
sudo systemctl reload nginx
```


#### Step 4: Access Your Website
Open your browser and navigate to your server's IP address (or `localhost` if using WSL):

```bash
http://<your-server-ip>
```
You should see your website.

### 4. Host a Website With a Domain
#### Step 1: Point Your Domain to the Server

1. Log in to your domain registrar's dashboard.
2. Update the DNS settings to point your domain to your server's IP address using an A record.

#### Step 2: Configure Nginx for the Domain

Edit the Nginx configuration file:
```bash
sudo nano /etc/nginx/sites-available/mywebsite
```

Update the `server_name` directive with your domain:
```bash
server {
    listen 80;
    server_name yourdomain.com www.yourdomain.com;

    root /var/www/mywebsite;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```
Save and exit, then reload Nginx:

```bash
sudo systemctl reload nginx
```

#### Step 3: Access Your Website

Open your browser and navigate to your domain:
```bash
http://yourdomain.com
```

### Troubleshooting

- Nginx not running: Check the status with `sudo systemctl status nginx` and review logs at `/var/log/nginx/error.log`.
- Firewall blocking traffic: Ensure ports 80 (HTTP) and 443 (HTTPS) are open.
- DNS issues: Verify your domain's DNS settings and wait for propagation.







