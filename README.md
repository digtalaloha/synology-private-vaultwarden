# Run a private, LAN access only instance Vaultwarden On A Synology NAS With Docker-Compose

## Description

This repository will provide you the details to run a private, LAN access only, instance of Vaultwarden, using docker-compose, on a Synology NAS.  The setup will be using the Caddy web server that will obtain a Let's Encrypt cert for a subdomain that is setup through Duck DNS.

I referenced the following links for much of the setup.

* [Running a private vaultwarden instance with Let’s Encrypt certs](https://github.com/dani-garcia/vaultwarden/wiki/Running-a-private-vaultwarden-instance-with-Let%27s-Encrypt-certs)
* [Docker-compose Caddy with DNS challenge](https://github.com/dani-garcia/vaultwarden/wiki/Using-Docker-Compose#caddy-with-dns-challenge)

## Directions

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
   a. Sign in or create a new account.
   b. Ceate a sub domain you would like to use (example http://vw-daloha.duckdns.org.
   c. Enter in the private IP address of your Synology NAS in the current ip box (example 10.0.4.33) and click update ip.
   d. Make note of the token (you'll need this later).
8. Download Caddy with DNS challenge module for Duck DNS from this link -> https://caddyserver.com/download.
   * Make sure to select the platform Linux amd64, select caddy-dns/duckdns, then click Download.
9. Upload the file to your Synology NAS under the /volume1/docker/synology-private-vaultwarden directory.  I used File Station to upload the file.
10. From the command line rename the file to caddy and set permissions if needed.
```
mv caddy_linux_amd64_custom caddy
chmod a+x caddy
```
