# Landing Page Server Setup Documentation

## Table of Contents

- [Project Overview](#project-overview)
- [Server Details](#server-details)
- [Setup Instructions](#setup-instructions)
- [SSL Configuration](#ssl-configuration)
- [Repository Structure](#repository-structure)
- [Maintenance](#maintenance)

## Project Overview

This project demonstrates the setup of a basic web server on AWS EC2 to host a landing page. The documentation covers server provisioning, web server configuration, and HTML page deployment.

## Server Details

| Component        | Specification           |
| ---------------- | ----------------------- |
| Server Type      | AWS EC2 t2.micro        |
| Operating System | Ubuntu Server 24.04 LTS |
| Web Server       | Nginx                   |
| Public IP        | 18.185.156.53  |

## Setup Instructions

### 1. Server Provisioning

#### AWS EC2 Instance Setup

1. Log into AWS Console
2. Navigate to EC2 Dashboard
3. Click "Launch Instance"
4. Configure your instance:
   ```
   Name: landing-page-server
   AMI: Ubuntu Server 24.04 LTS
   Instance Type: t2.micro
   Key pair: Create new key pair "call-it-whatever-you-want"
   ```

#### Security Group Configuration

Create a new security group with the following rules:

| Type | Protocol | Port Range | Source    |
| ---- | -------- | ---------- | --------- |
| HTTP | TCP      | 80         | 0.0.0.0/0 |
| SSH  | TCP      | 22         | 18.185.156.53   |

### 2. Server Configuration

#### SSH Access Setup

```bash
# Set key pair permissions
chmod 400 call-it-whatever-you-want.pem

# Connect to your instance
ssh -i call-it-whatever-you-want.pem ubuntu@[ec2-public-ip]
```

#### Install and Configure Nginx

```bash
# Update system packages
sudo apt update
sudo apt upgrade -y

# Install Nginx
sudo apt install nginx -y

# Start and enable Nginx
sudo systemctl start nginx
sudo systemctl enable nginx
```

### 3. Landing Page Deployment

#### Create Landing Page Content

Create a new file at `/var/www/html/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Welcome to Abram's Landing Page</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        line-height: 1.6;
        margin: 0;
        padding: 20px;
        max-width: 800px;
        margin: 0 auto;
      }
      h1 {
        color: #333;
        border-bottom: 2px solid #eee;
        padding-bottom: 10px;
      }
      .bio {
        background: #f9f9f9;
        padding: 20px;
        border-radius: 5px;
        margin-top: 20px;
      }
    </style>
  </head>
  <body>
    <h1>Welcome to Abram's Landing Page</h1>

    <div class="project-description">
      <h2>About This Project</h2>
      <p>
        This landing page demonstrates my capability to deploy and manage web
        applications using cloud infrastructure and modern web technologies. :-)
      </p>
    </div>

    <div class="bio">
      <h2>About Me</h2>
      <p>
        I'm <a href="https://github.com/aybruhm" target="_blank">Abram</a>, a product software engineer with 5 years of industry
        experience spanning full-time, contract and freelance roles. I thrive on
        designing and developing the intricate components of software that
        enhance customer interactions with the product. My expertise extends
        across various functions such as Backend (core), DevOps, Support, and
        occasionally Frontend.
      </p>
    </div>
  </body>
</html>
```

#### Set File Permissions

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

## SSL Configuration

### Installing SSL Certificate (Optional)

After running the following commands, see here for documentation on configuring the server to make use of the SSL certificate.

```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx -y

# Get SSL certificate
sudo certbot --nginx -d your-domain.com

# Verify auto-renewal
sudo systemctl status certbot.timer
```

## Repository Structure

```
.
├── README.md
├── screenshots/
│   ├── landing-page.png
│   └── server-status.png
└── src/
    └── index.html
```

## Maintenance

### Regular Tasks

- Update system packages:

  ```bash
  sudo apt update && sudo apt upgrade -y
  ```

- Monitor server status:

  ```bash
  sudo systemctl status nginx
  ```

- Check error logs:
  ```bash
  sudo tail -f /var/log/nginx/error.log
  ```


## Notes

For security reasons, I cannot share the server's PEM key, as the github repository is public.

```
Public Domain Name: altschool.abram.tech 
Last Updated: Sat, Dec 21, 2024  
Maintainer: Abram <@aybruhm>
```
