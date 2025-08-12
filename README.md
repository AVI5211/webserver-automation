
markdown
Copy
Edit
# Apache HTTP Server Setup on Ubuntu (EC2)

This guide walks through installing Apache HTTPD on an Ubuntu-based EC2 instance, deploying a simple HTML page, and setting up SSH key-based authentication.

---

## 1. Prerequisites

On a **new machine**, update packages and install required tools:

```bash
sudo apt update && sudo apt install -y unzip awscli
2. Install Apache HTTPD
Run the following commands to install and start Apache:

bash
Copy
Edit
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
3. Check Web Server Status
To verify Apache is running:

bash
Copy
Edit
systemctl status apache2
4. Deploy a Sample HTML Page
Create an index.html file:

html
Copy
Edit
<!DOCTYPE html>
<html>
<head>
<title>Hello AWS</title>
</head>
<body>
<h1>Hello from EC2 & Apache!</h1>
</body>
</html>
Replace the default Apache index page:

bash
Copy
Edit
sudo chown ubuntu:ubuntu /var/www/html
sudo chown -R ubuntu:ubuntu /var/www/html
sudo chmod -R 755 /var/www/html
Copy your index.html into /var/www/html/:

bash
Copy
Edit
cp index.html /var/www/html/
5. Test in Browser
Open your browser and navigate to:

cpp
Copy
Edit
http://<EC2_PUBLIC_IP>
Replace <EC2_PUBLIC_IP> with your EC2 instance's public IP address.

6. SSH Key Automation
Generate a new SSH key pair:

bash
Copy
Edit
ssh-keygen -t rsa -b 4096 -f deploy_key
cat deploy_key.pub
Add the public key to the remote server:

bash
Copy
Edit
mkdir -p ~/.ssh
echo "<PASTE_PUBLIC_KEY_HERE>" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
Encode the private key in Base64 (useful for automation pipelines):

bash
Copy
Edit
base64 -w 0 deploy_key > deploy_key.base64
cat deploy_key.base64
