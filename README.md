# setup
knowledge base to setup various things on ubuntu

## Turn off suspend on Lid close

- sudo nano /etc/systemd/logind.conf
-  add line `HandleLidSwitch=ignore`

## Install docker

- sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
- sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
- sudo apt-get update
- sudo apt-get install -y docker-ce docker-ce-cli containerd.io
- sudo usermod -aG docker $USER

### References

- https://docs.docker.com/install/linux/docker-ce/ubuntu/
- https://docs.docker.com/install/linux/linux-postinstall/

## Install peek

Peek records your screen and generates gif.

- sudo add-apt-repository ppa:peek-developers/stable
- sudo apt update
- sudo apt upgrade
- sudo apt install peek

## Random background on Ubuntu 18.04

- sudo nano /usr/local/bin/unsplash.sh
- sudo chmod +x /usr/local/bin/unsplash.sh
- crontab -e

### file contents

```
#/bin/bash

wget -O /tmp/wallpaper.jpg https://picsum.photos/1920/1080/?random
gsettings set org.gnome.desktop.background picture-uri file:///tmp/wallpaper.jpg
```
### Cron timing

New wallpaper every 30 minutes: `*/30 * * * * /usr/local/bin/unsplash.sh`

## Must have tweak tool for Ubuntu 18.04

- sudo apt install gnome-tweak-tool

## Crendential helper for docker

- wget https://github.com/docker/docker-credential-helpers/releases/download/v0.6.0/docker-credential-pass-v0.6.0-amd64.tar.gz
- tar -xf docker-credential-pass-v0.6.0-amd64.tar.gz
- sudo mv docker-credential-pass /usr/local/bin/docker-credential-pass
- sudo apt install -y gpg pass
- gpg --full-generate-key
- pass init 7EF78FB814C2FEFEFEF1DF76B7DCB97DB7174C52 # copy id from the former command
- nano ~/.docker/config.json

### Config.json contents

```
"credsStore": "pass"
```

### References

- https://github.com/docker/docker-credential-helpers/issues/102

## Install Certbot

- sudo apt-get update -y
- sudo apt-get install -y software-properties-common
- sudo add-apt-repository -y universe
- sudo add-apt-repository -y ppa:certbot/certbot
- sudo apt-get update -y
- sudo apt-get install -y certbot 
- sudo certbot certonly

### Cron job

run every sunday `0 0 * * 0 certbot certonly`

## Install docker registry

- docker secret create domain.crt /etc/letsencrypt/live/<DOMAIN>/fullchain.pem
- docker secret create domain.key /etc/letsencrypt/live/<DOMAIN>/privkey.pem
- sudo mkdir /auth
- docker run --entrypoint htpasswd registry:2 -Bbn <USER> <PASSWORD> > /auth/htpasswd
- mkdir -p /mnt/registry
- docker service create --name registry --secret domain.crt --secret domain.key --mount type=bind,src=/mnt/registry,dst=/var/lib/registry --mount type=bind,src=/auth/htpasswd,dst=/auth/htpasswd -e "REGISTRY_AUTH=htpasswd" -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd -e REGISTRY_HTTP_ADDR=0.0.0.0:5000 -e REGISTRY_HTTP_TLS_CERTIFICATE=/run/secrets/domain.crt -e REGISTRY_HTTP_TLS_KEY=/run/secrets/domain.key --publish published=5000,target=5000 registry:2

## Install GO Athens

- mkdir -p /mnt/athens
- docker service create --mount type=bind,src=/mnt/athens,dst=/var/lib/athens -e ATHENS_DISK_STORAGE_ROOT=/var/lib/athens -e ATHENS_STORAGE_TYPE=disk --name athens-proxy --publish published=3000,target=3000 gomods/athens:latest
