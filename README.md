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
