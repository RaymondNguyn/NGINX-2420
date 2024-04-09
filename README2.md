# PART 2

## FIREWALL SETUP-UFW
Assuming we are using the same file from before. To install a fire wall we will be using UFW install with `sudo pacman -S ufw` 
Reset your droplet \
We will then allow the follow ports  through with `ufw allow [port]` 
- `ufw allow 22` Our ssh
- `ufw allow 'NGINX'` Our Nginx
- `uf allow 8080` Our backend server

## SFTP
Now we will upload the backend file from our local PC with SFTP In the terminal enter ` sftp -i /path/tp/authkey [user]@[ipaddr]` Once in whatever command on linux to navigate the local pc you just add an l `lcd` locally change directory. Change directory to the backend file on your local pc and use `put [backend-server]` to upload it to your remote server. Once that is done `exit`.

## Setting up the backend
Now that the backend is uploaded on the remote server we have to move it to the correct directory move it with `sudo mv hello-server /usr/local/bin`
Once that is done we will now create a service file to run the backend.

### Service file
Change directories to `cd /etc/systemd/system`. Now create a service file with `sudo vim [service-file-name.service]` 

Paste the following:
```
[Unit]
Description=Backend for nginx
After=network.target

[Service]
ExecStart=/usr/local/bin/hello-server
Restart=on-failure


[Install]
WantedBy=multi-user.target
```
After pasting that `sudo systemctl daemon-reload` and then `sudo systemctl start backend.service` This will then get reversed proxied by NGINX check to see if the service is running and active with `sudo systemctl status backend.service`

### NGINX Reverse Proxy

Now that the above is done change directory to our sites avalible file at `/etc/nginx/sites-available` we are then going to edit the `nginx-2420.conf` file created from part 1. `sudo vim nginx-2420.conf` and then \

Paste the following into the server block from last time

```
    location /hey {
        # Define the reverse proxy settings
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        }

    location /echo {
        # Define the reverse proxy settings
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
```

The file should look like this in the end:
```
server {
    listen 80;
    listen [::]:80;
    server_name 146.190.162.154;
    root /web/html/nginx-2420;
    location / {
        index index.php index.html index.htm;
    }

    location /hey {
        # Define the reverse proxy settings
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        }

    location /echo {
        # Define the reverse proxy settings
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        }
}
```
After pasting the following restart your nginx with `sudo systemctl restart nginx`

### Testing
In the webbrower of choice enter the droplet ip as you can see the page from the last part. \
Now to test if what we did worked enter `http://[ipaddress]/hey` you should see a little `Hey there` \
You could also test in the terminal with:

The following two will return the raw text of each
```
curl http://146.190.162.154/ 
```

```
curl http://146.190.162.154/hey
```

The following is a POST request that will echo back in the terminal
```
curl -X POST -H "Content-Type: application/json" \
  -d '{"message": "Hello from your server"}' \
  http://[Ipaddress]/echo
```

# END
That is the end of this tutorial by Raymond Nguyen for Assignment 3 Part 2
