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

`ufw status`

`ufw allow 'Nginx Full'`

`ufw delete allow 'Nginx HTTP'`

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

`certbot --nginx -d example.com -d www.example.com`

## Disable SSH Password Login

`nano /etc/ssh/ssh_config`

`ChallengeResponseAuthentication no`

`PasswordAuthentication no`

`UsePAM no`

## Flask

### Nginx

`nano /etc/nginx/sites-available/example.com`

<pre>
server {
    listen 80;
    server_name _;

    location / {
        include proxy_params;
        proxy_pass http://127.0.0.1:5000;
    }

    location /static/ {
        alias <path-to-your-application>/static/;
        expires 30d;
    }

    location /socket.io {
        include proxy_params;
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_pass http://127.0.0.1:5000/socket.io;
    }
}
</pre>

`nginx -t`

`systemctl reload nginx`

### Systemd Service

`nano /etc/systemd/system/flask.service`

<pre>
[Unit]
Description=uWSGI instance to serve flask-app
After=network.target

[Service]
User=user
Group=www-data
WorkingDirectory=/home/user/flask-app
Environment="PATH=/home/user/flask-app/.venv/bin"
ExecStart=python3 /home/user/flask-app/wsgi.py

[Install]
WantedBy=multi-user.target
</pre>

`systemctl start flask`

`systemctl enable flask`

`systemctl status flask`
