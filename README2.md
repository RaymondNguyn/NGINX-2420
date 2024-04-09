# PART 2

## FIREWALL SETUP-UFW
Assuming we are using the same file from before. To install a fire wall we will be using UFW install with `sudo pacman -S ufw` \
Reset your droplet \
We will then allow the follow ports  through with `ufw allow [port]` \
- `ufw allow 22` Our ssh
- `ufw allow 'NGINX'` Our Nginx
- `uf allow 8080` Our backend server

## SFTP
Now we will upload the backend file from our local PC with SFTP In the terminal enter ` sftp -i /path/tp/authkey [user]@[ipaddr]`
