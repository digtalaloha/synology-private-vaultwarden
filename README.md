# Run A Private, LAN Access Only, Instance Of Vaultwarden On A Synology NAS With Docker-Compose And Duck DNS

## Description

This repository will provide you the details to run a private, LAN access only, instance of Vaultwarden, using docker-compose, on a Synology NAS.  The setup will be using the Caddy web server that will obtain a Let's Encrypt certificate for a subdomain that is setup through Duck DNS.

Because Synology NASs use both ports 80 and 443 for DSM we'll need to use a custom port number when accessing Vaultwarden.  The url to access Vaultwarden would look something like https://vw-daloha.duckdns.org:8443.  See step 12 in the Setup Steps below to set the custom ports.

I referenced the following links for much of the setup description below.

* [Running a private vaultwarden instance with Let’s Encrypt certs](https://github.com/dani-garcia/vaultwarden/wiki/Running-a-private-vaultwarden-instance-with-Let%27s-Encrypt-certs)
* [Docker-compose Caddy with DNS challenge](https://github.com/dani-garcia/vaultwarden/wiki/Using-Docker-Compose#caddy-with-dns-challenge)

## Directions

### YouTube Video
If you would like directions in visual format then check out my YouTube video that runs through the setup.
* [Run Your Own Private Vaultwarden Instance With Docker-Compose And Git On Your Synology NAS](https://youtu.be/pH_LZVfuSWo)

### Prerequistes
On your Synology NAS you'll need to do the following:
1. Install the Docker package.
2. Install the Git Server package.
3. Enable SSH.

### Setup Steps 
1. SSH into your Synology NAS.
```
ssh <admin account>@<IP address of Synology NAS>
```
2. Change directory into /volume1/docker. 
```
cd /volume1/docker
```
4. Clone this repository.
```
sudo git clone https://github.com/digtalaloha/synology-private-vaultwarden.git
```
5. Change directory into the newly created synology-private-vaultwarden directory.
```
cd synology-private-vaultwarden
```
6. Create host volume mount points that will be used by the Vaultwarden and Caddy containers.
```
mkdir vw-data caddy-config caddy-data
```
7. Go to [Duck DNS](https://www.duckdns.org) and do the following:
   1. Sign in or create a new account.
   2. Ceate a sub domain you would like to use (example http://vw-daloha.duckdns.org).
   3. Enter in the private IP address of your Synology NAS in the current ip box (example 10.0.4.33) and click update ip.
   4. Make note of the token (you'll need this later).
8. Download Caddy with DNS challenge module for Duck DNS from this link -> [Caddy Download Page](https://caddyserver.com/download).
   * Make sure to select the platform Linux amd64, select caddy-dns/duckdns, then click Download.
9. Upload the Caddy file you just downloaded to your Synology NAS to the /volume1/docker/synology-private-vaultwarden directory.  
   * I used File Station to upload the file.
10. While still in File Station rename the file to **caddy**.
11. From the command line confirm the permissions are as follow and if not run the second command listed.
```
ls -la caddy (this should return -rwxrwxrwx+ for the permissions)
chmod a+x caddy (run this if the permissions isn't what is listed above)
```
12. Edit the .env file with the specifics of your setup.  
  * Note - port 8080 and 8443 are the host (Synology NAS) ports that will be mapped to the Caddy ports 80 and 443 respectively.  These are the ports I used in my setup.
  * These entries will be used to auto populate the docker-compose.yaml file, which will in turn auto populate the Caddyfile.
```
CADDY_HTTP_PORT=8080
CADDY_HTTPS_PORT=8443
DUCKDNS_DOMAIN="https://YOUR_DUCK_DNS_SUB_DOMAIN.duckdns.org"
EMAIL="YOUR_EMAIL_ADDRESS"
DUCKDNS_TOKEN="YOUR_DUCK_DNS_TOKEN"
```
13. Create and start up Vaultwarden with the Caddy web server.
```
sudo docker-compose up -d
```
14. You should now be able to access your private, LAN access only Vaultwarden docker container.
```
Go to https://YOUR_DUCKDSN_SUB_DOMAIN.duckdns.org:CADDY_HTTPS_PORT (Example - https://vw-daloha.duckdns.org:8443)
```
