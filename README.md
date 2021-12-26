# Setup Linode Server / Ubuntu 20.04

`apt update`
`apt upgrade`

## 1. Pi-Hole

[Pi-Hole](https://pi-hole.net/ "Pi-Hole")

### Install Pi-Hole

`curl -sSL https://install.pi-hole.net | bash`

### Update Pi-Hole

`pihole -up`

## 2. PiVPN

[PiVPN](https://pivpn.io/ "PiVPN")

### Install PiVPN

`curl -L https://install.pivpn.io | bash`

## 3. nginx

`apt install nginx`

### Firewall

`ufw allow 'Nginx Full`

## 4. PHP-FPM

`apt install php-fpm php-cli php-curl php-mysql php-curl php-gd php-mbstring php-pear`

## 5. MySQL

`apt install mysql-server`

### MySQL Login

`mysql -u root`

### Create Database

`CREATE DATABASE database;`

`CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';`

`GRANT ALL PRIVILEGES ON database.* TO 'username'@'localhost';`

`FLUSH PRIVILEGES;`

## 6. Nextcloud

`wget https://download.nextcloud.com/server/installer/setup-nextcloud.php`

## Certbot

`apt install certbot python3-certbot-nginx`
`certbot --nginx`
