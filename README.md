# setup
knowledge base to setup various things on ubuntu

## Install docker

- sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
- sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
- sudo apt-get update
- sudo apt-get install docker-ce docker-ce-cli containerd.io
- sudo usermod -aG docker $USER

### References

- https://docs.docker.com/install/linux/docker-ce/ubuntu/
- https://docs.docker.com/install/linux/linux-postinstall/

## Install peek

Peek records your screen and generates gif.

- sudo add-apt-repository - ppa:peek-developers/stable
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
- sudo apt install gpg pass
- gpg --full-generate-key
- pass init 7EF78FB814C2FEFEFEF1DF76B7DCB97DB7174C52 # copy id from the former command
- nano ~/.docker/config.json

### Config.json contents

```
"credsStore": "pass"
```

### References

- https://github.com/docker/docker-credential-helpers/issues/102