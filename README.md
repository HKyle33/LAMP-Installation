# LAMP Stack (Linux + Apache + MariaDB + PHP) Installation with Ubuntu 16.04/18.04
ICST241 Partial Project Completion  
Group 3: Baldo, Bracero, Lacerno, Orendain, Severo  
Operating System: Ubuntu 16.04/18.04 Desktop

### Roles
Baldo - Documentation, Installation, and Live Demonstration  
Bracero - Live Demo, Documentation, Tutorial Recording, and Installation  
Lacerna - Live Demo, Recording, Editing, and Installation  
Orendain - Live Demonstration, Recording and Installation  
Severo - Live Demonstration, Installation, and Recording  

This document assumes that the computer has a fresh install of the operating system. No proxy
settings has been configured. If not, follow the section below.

## Installing Ubuntu 16.04/18.04 Desktop Using Oracle VM VirtualBox
1. Click “New.”
2. Specify the name of the operating system. (e.g. Ubuntu)
3. Click Next.
4. Specify the amount of memory (RAM) in megabytes to be allocated to the virtual machine. (e.g. 1024 mb)
5. Click Next.
6. Choose “Create a virtual hard disk now.”
7. Click Next.
8. Choose “VDI (VirtualBox Disk Image).”
9. Click Next.
10. Choose either dynamically allocated or fixed size hard disk file to create the new virtual hard disk.
11. Leave the file location and size section as is.
12. Click “Create.”
13. After the VDI is created, go to Settings > Storage
14. Choose your Ubuntu ISO and place it under Controller: IDE.
15. Click OK.
16. Choose Start button.
17. Follow the succeeding steps and choose your preferences

**Note:**
Enter `sudo su` to run the scripts to the following steps with root privileges.  
Before you do everything else, run `apt-get update` as well to update the package lists for upgrades for packages
that need upgrading.

## Installing and Configuring Apache Web Server
Installation of Apache
```
apt-get install apache2
```
Check for syntax errors if ServerName is not global
```
apache2ctl configtest
```
Edit apache2.conf using a text editor (in this case, it's nano) to set the ***ServerName***.
```
nano /etc/apache2/apache2.conf
```
Enter this line in apache2.conf. `<address>` is your primary domain name (your IPV4 address). Do not forget to save.
```
ServerName <address>
```
Check if there are syntax errors in the file with `apache2ctl configtest`  

To implement changes, restart Apache
```
systemctl restart apache2
```

#### Adjusting the Firewall to Allow Web Traffic
Enter `ufw app list` to look at available applications.

Run this command to show that it enables traffic to ports 80 and 443
```
ufw app info "Apache Full"
```
Run this command to allow incoming traffic
```
ufw allow in "Apache Full"
```
To make ***www-data*** as the owner of the web directory
```
chown www-data /var/www/html
```
At this point, you already have succesfully installed and configured Apache in your system.

## Installing MariaDB 10
Command for the installation of both server and client
```
apt-get -y install mariadb-server mariadb-client
```
Enter `systemctl status mariadb` to check MariaDB status if it is running. You can manually start MariaDB by entering `systemctl start mariadb`  
Set up root password and preferences of your choice for MariaDB
```
mysql_secure_installation
```

## Installing PHP
Run this code to install PHP if The version of your Ubuntu machine is in **16.04** as `php-mcrypt` is already obsolete. This will install the PHP version prior to 7.2
```
apt-get install php libapache2-mod-php php-mcrypt php-mysql
```
Run this code if the version of your Ubuntu machine is in **18.04**. This will install PHP 7.2 on your system.
```
apt install php libapache2-mod-php php-mysql
```
Enter this code to open dir.conf with a text editor
```
nano /etc/apache2/mods-enabled/dir.conf
```
Replace the entire line of `DirectoryIndex` in the file to this line
```
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml
index.htm
```
To implement changes, we need to restart Apache by entering `systemctl restart apache2`. Check the status of Apache by entering `sudo systemctl status apache2`

#### Test if PHP is working
Create a PHP file called info.php using a text editor (nano) by typing this code
```
nano /var/www/html/info.php
```
Type in `<?php phpinfo(); ?>` and don't forget to save.  
After that, go to your browser of choice and type `127.0.0.1/info.php` in the address bar. ***PHP*** is working if you see relevant contents regarding the system.

## OPTIONAL: Installing LAMP under ADNU CSLAB (Ubuntu 18.04 Virtual Machine)
If you are planning to install the stack under ADNU's CSLABs, we first have to set all of the
system proxy servers to `172.16.4.254` and port to `3128` in the network settings of your virtual operating system.

#### Setting up the proxy for the terminal to allow apt installation
Create a new config file called `proxy.conf` using this code
```
touch /etc/apt/apt.conf.d/proxy.conf
```
Open the proxy.conf file with a text editor of your choice (In this case, it's nano again)
```
sudo gedit /etc/apt/apt.conf.d/proxy.conf
```
Add these lines to the `proxy.conf` file to set your HTTP and HTTPS proxy
```
Acquire::http::Proxy "http://172.16.4.254:3128/";
Acquire::https::Proxy "http://172.16.4.254:3128/";
```

## Conclusion
Considering that you have Apache, MariaDB, and PHP installed and running on a Linux environment, we can already say that you have fulfilled all the necessary requirements which means that LAMP is already installed.
