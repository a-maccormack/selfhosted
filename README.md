# Selfhosted

Selfhosted home server configuration. Own your data without giving up privacy or locking yourself into a service you don't control.

## Containers Used:

### Networking
* [Nginx](https://hub.docker.com/_/nginx) (web server / reverse proxy)
* [PiHole](https://hub.docker.com/r/pihole/pihole/) (DNS with built-in ad-blocking)

### Media
* [Jellyfin](https://hub.docker.com/r/linuxserver/jellyfin) (media server)

### Services
* [Homer](https://hub.docker.com/r/b4bz/homer) (static home page)
* [Nextcloud](https://hub.docker.com/r/linuxserver/nextcloud) (cloud platform)


## Usage:
Clone repository: 
```
git clone https://github.com/a-maccormack/selfhosted
```

Move folder contents into your ```/home/<youruser>``` directory:
```
cd selfhosted && mv -r * /home/<youruser>
```
Install docker and docker-compose:
```
sudo apt install docker.io
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
Modify docker user permissions for rootless access:
```
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker
```
Set up Nginx, Jellyfin, Homer, and Nextcloud:
```
#docker-compose 
#Do for all services: [nginx, jellyfin, homer, nextcloud]
cd ~/docker-compose-files/<service> && docker-compose up -d
```
Docker-compose Pihole:
```
#docker-compose pihole
cd ~/docker-compose-files/pihole && docker-compose up -d

#set password
docker exec pihole pihole -a -p <your password>
```
Remember to change your DNS to use PiHole

## Useful Tools
* Resource Monitoring: ```sudo apt install htop```
* System Information: ```sudo apt install neofetch```
* OhMyZsh: ```sudo apt install szh && sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"```





