# Installing and Configuring the Apache Web Server

_Apache, Self-Signed SSL Certificate, other than 80/443._

To install the Apache web server on a Linux system, you'll typically use a package manager specific to your distribution. Here are the commands for some popular Linux distributions:

## Ubuntu / Debian:

```
sudo apt update
sudo apt install apache2
```

## CentOS / RHEL:

```
sudo yum install httpd
```

## Fedora:

```
sudo dnf install httpd
```

## OpenSUSE:

```
sudo zypper install apache2
```

## Arch Linux:

```
sudo pacman -S apache
```

## Alpine Linux:

```
sudo apk add apache2
```

After running the appropriate command for your distribution, the package manager will download and install the Apache web server along with any necessary dependencies.

Once the installation is complete, you can start the Apache service using the following command:

```
sudo systemctl start apache2   # Ubuntu / Debian
sudo systemctl start httpd     # CentOS / RHEL, Fedora
```

## To enable Apache to start on boot:

```
sudo systemctl enable apache2   # Ubuntu / Debian
sudo systemctl enable httpd     # CentOS / RHEL, Fedora
```

You can then open a web browser and enter your server's IP address to see the default Apache welcome page. If everything is set up correctly, you'll be able to access your server via a web browser.

Remember to adjust your firewall settings if needed to allow traffic on port 80 (HTTP) so that your server can be accessed over the network.

Please note that these commands assume that you have appropriate privileges (usually via sudo) to install packages on your system.

To set up a self-signed SSL certificate and configure Apache to use a port other than the default 80 (HTTP) and 443 (HTTPS), you'll need to follow these steps:

### Step 1: Generate a Self-Signed SSL Certificate

1. Open a terminal on your Linux system.
2. Navigate to the directory where Apache stores its SSL certificates. This is often /etc/ssl or a similar location. If it doesn't exist, you may need to create it.
3. Generate a self-signed SSL certificate and private key using the following OpenSSL command:

```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
```

Follow the prompts to provide the necessary information (e.g., country, state, organization, etc.). Leave the "Common Name" blank or enter the domain name of your server if you have one.

### Step 2: Configure Apache for SSL

Enable the SSL module in Apache:

```
sudo a2enmod ssl
```

Open the SSL configuration file for editing. This file might be named ssl.conf or default-ssl.conf and is typically located in /etc/apache2/sites-available/.

```
sudo nano /etc/apache2/sites-available/default-ssl.conf
```

Inside the <VirtualHost> block, add or modify the following lines to specify the SSL certificate and key files:

```
SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
```

Optionally, you can configure other SSL settings such as the server name, protocol, and cipher suite. Save and close the file.

### Step 3: Configure Apache to Use a Different Port

Open the Apache configuration file for editing. This file is typically named httpd.conf or apache2.conf and is usually located in /etc/httpd/ or /etc/apache2/.

```
sudo nano /etc/httpd/conf/httpd.conf   # For CentOS / RHEL
sudo nano /etc/apache2/apache2.conf    # For Ubuntu / Debian
```

Locate the section where ports are defined. There will be lines like Listen 80 for HTTP and Listen 443 for HTTPS.

Add a new line to specify the port you want to use, for example:

```
Listen 8080
```

### Step 4: Update Firewall Rules
If you're using a firewall, make sure to open the new port you specified (e.g., 8080) to allow incoming traffic.

### Step 5: Restart Apache
Finally, restart the Apache service for the changes to take effect:

```
sudo systemctl restart apache2   # Ubuntu / Debian
sudo systemctl restart httpd     # CentOS / RHEL, Fedora
```

Now, your Apache web server should be using a self-signed SSL certificate and listening on the port you specified (e.g., 8080). 

Keep in mind that users will need to specify the port in the URL (e.g., https://yourdomain.com:8080) to access your site securely.
