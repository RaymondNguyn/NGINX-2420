# PART 2

## FIREWALL SETUP-UFW
Assuming we are using the same file from before. To install a fire wall we will be using UFW install with `sudo pacman -S ufw` \
Reset your droplet \
We will then allow the follow ports  through with `ufw allow [port]` \
- `ufw allow 22` Our ssh
- `ufw allow 'NGINX'` Our Nginx
- `uf allow 8080` Our backend server

## SFTP
Now we will upload the backend file from our local PC with SFTP In the terminal enter ` sftp -i /path/tp/authkey [user]@[ipaddr]` Once in whatever command on linux to navigate the local pc you just add an l `lcd` locally change directory. Change directory to the backend file on your local pc and use `put [backend-server]` to upload it to your remote server. Once that is done `exit`.

## Setting up the backend
Now that the backend is uploaded on the remote server we have to move it to the correct directory move it with `sudo mv hello-server /usr/local/bin`
Once that is done we will now create a service file to run the backend.

### Service file
Change directories to `cd /etc/systemd/system`. Now create a service file with `sudo vim [service-file-name.service]` \

Paste the following:
```
[Unit]
Description=Backend for NGINX
After=network.target

[Service]
User=[
WorkingDirectory=/home/ray/nginx-server
Environment=PORT=8080
ExecStart=/home/ray/env/nginx/bin/gunicorn --workers 3 --bind 0.0.0.0:8000 wsgi:app

[Install]
WantedBy=multi-user.target
```
