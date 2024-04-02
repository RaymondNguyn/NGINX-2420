# NGINX-2420
Assignment 3 for linux 2420 w/ Arch Linux Server

# Prerequisite
Assuming we already have an Arch Linux Server droplet set-up aswell as sudo access in your system. Proceed with the tutorial:
You have to install the following packages:
- VIM
- NGINX


## CHECK/UPDATE ARCH
Before we install any packages make sure our system is up to date with the following:
`sudo pacman -Syu`
It is important to keep our distro up-to-date to make sure everything works with eachother without conflicts

## VIM
vim is a text editor we will need this to edit files on our linux distro aswell as configure our nginx server
To install vim in arch linux run `sudo pacman -S vim`

## NGINX
Nginx is a webserver software used to host and serve webpages
To install NGINX in arch linux run `sudo pacman -S nginx`

# NGINX CONFIGURATIONS
The default NGINX directory is located at `/etc/nginx/` while the main configuration file is located at `/etc/nginx/nginx.conf`

## Example of what the NGINX Conf file looks like
```
user http;
worker_processes auto;
worker_cpu_affinity auto;

events {
    multi_accept on;
    worker_connections 1024;
}

http {
    charset utf-8;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    server_tokens off;
    log_not_found off;
    types_hash_max_size 4096;
    client_max_body_size 16M;

    # MIME
    include mime.types;
    default_type application/octet-stream;

    # logging
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log warn;

    # load configs
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

## Serverblocks
Server blocks are to host more than one domain on a single server

In the `/etc/nginx/nginx.conf` file paste the following in the `http{}`

`listen 80` listen for HTTP request on port 80\
`server_name` is where your domain name goes\
`root` is the directory where the files for `server_name` will be\
`location /` Defines how nginx should handle request for ('/') in the URL in this case nginx is look for the following: `index.php index.html index.htm`

### Server Block Config
```
server {
    listen 80;
    listen [::]:80;
    server_name domainname1.tld;
    root /usr/share/nginx/domainname1.tld/html;
    location / {
        index index.php index.html index.htm;
    }
}

server {
    listen 80;
    listen [::]:80;
    server_name domainname2.tld;
    root /usr/share/nginx/domainname2.tld/html;
    ...
}
```


