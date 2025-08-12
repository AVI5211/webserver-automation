
# Webserver Automation

A comprehensive guide for automating Apache HTTP server deployment on Ubuntu EC2 instances with SSH key authentication.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Deployment](#deployment)
- [Testing](#testing)
- [SSH Key Automation](#ssh-key-automation)
- [Troubleshooting](#troubleshooting)

## Overview

This project provides step-by-step instructions for:
- Installing Apache HTTP server on Ubuntu-based EC2 instances
- Deploying a simple HTML page
- Setting up SSH key-based authentication for secure automation
- Configuring proper file permissions and ownership

## Prerequisites

Before starting, ensure you have:
- An Ubuntu-based EC2 instance with sudo privileges
- Basic understanding of Linux command line
- SSH access to your EC2 instance

On a **new machine**, update packages and install required tools:

```bash
sudo apt update && sudo apt install -y unzip awscli
```

## Installation

### Install Apache HTTPD

Run the following commands to install and start Apache:

```bash
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

### Verify Installation

To verify Apache is running:

```bash
systemctl status apache2
```

## Configuration

### Set Up File Permissions

Configure proper ownership and permissions for the web directory:

```bash
sudo chown ubuntu:ubuntu /var/www/html
sudo chown -R ubuntu:ubuntu /var/www/html
sudo chmod -R 755 /var/www/html
```

## Deployment

### Create HTML Page

Create an `index.html` file with the following content:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Hello AWS</title>
</head>
<body>
    <h1>Hello from EC2 & Apache!</h1>
</body>
</html>
```

### Deploy to Web Server

Copy your `index.html` file to the web root directory:

```bash
cp index.html /var/www/html/
```

## Testing

### Browser Test

Open your browser and navigate to:

```
http://<EC2_PUBLIC_IP>
```

Replace `<EC2_PUBLIC_IP>` with your EC2 instance's public IP address.

## SSH Key Automation

### Generate SSH Key Pair

Create a new SSH key pair for automated deployments:

```bash
ssh-keygen -t rsa -b 4096 -f deploy_key
cat deploy_key.pub
```

### Configure Server

Add the public key to the remote server:

```bash
mkdir -p ~/.ssh
echo "<PASTE_PUBLIC_KEY_HERE>" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

### Prepare for CI/CD

Encode the private key in Base64 for use in automation pipelines:

```bash
base64 -w 0 deploy_key > deploy_key.base64
cat deploy_key.base64
```

## Troubleshooting

### Common Issues

- **Apache not starting**: Check if port 80 is already in use
- **Permission denied**: Ensure proper file ownership and permissions
- **SSH connection failed**: Verify security group settings allow SSH (port 22)
- **Web page not loading**: Check if security group allows HTTP (port 80)

### Useful Commands

```bash
# Check Apache status
sudo systemctl status apache2

# View Apache error logs
sudo tail -f /var/log/apache2/error.log

# Restart Apache service
sudo systemctl restart apache2

# Check open ports
sudo netstat -tlnp
```
