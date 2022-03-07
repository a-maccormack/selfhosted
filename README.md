# Selfhosted

Selfhosted home server configuration. Own your data without giving up privacy or locking yourself into a service you don't control.

## Operating system:
[Ubuntu Server 20.04.4 LTS](https://releases.ubuntu.com/20.04.4/ubuntu-20.04.4-live-server-amd64.iso)

## SSH:
### Generating SSH Keys:
Using a clear text password is never a good idea, since the password is not encrypted in transit, and can be exposed on a hostile network. Permitting password login is a threat. 

In order to generate an SSH key, type the following on your local machine:

```bash
#On local machine
ssh-keygen -t rsa -b 4096
```

Press enter when asked for a key location, and add a password of choice to your key. 

Next, connect to your server by typing:
```bash
#On local machine
ssh root@<your-ip>
```

### Creating a non-root user:
Exposing root login on an ssh server is a security threat. To create a new user, type:
```bash
#On server
useradd -G sudo -m <your-username> -s /bin/bash
passwd <your-username>
```

### Copying SSH key to server:
On your local machine, type:
```
#On local machine
ssh-copy-id <your-username>@<your-ip>
```
### SSH configuration:
Go back to your server's terminal window, and edit the ssh config by typing:
```bash
#On server
nano /etc/ssh/sshd_config
```

The config file parameters that you should change are listed below:
```bash
Port <port-other-than-default>
PasswordAutentication no
PermitRootLogin no
```

Make sure to save the file once you have changed the configuration, and restart the ssh service by using:
```bash
systemctl restart sshd
```

Once this is done, make sure to check that password login is no longer permitted.

### Adding SSH Alias
Create a file on your local machine's ssh configuration directory called config:
```
nano ~/ssh/config
```

The file should look like this:

```bash
Host <your-ssh-alias>
  User <your-non-root-user>
  Port <your-ssh-port>
  IdentityFile ~/.ssh/id_rsa
  HostName <your-server-ip>
```

Once you save your file, you'll be able to login to your server by using the following command:

```bash
ssh <your-ssh-alias>
```

### Removing unwanted system information on ssh login:
```bash
touch .hushlogin
```


## Containers Used:

### Networking
* [Nginx](https://hub.docker.com/_/nginx) (web server / reverse proxy)
* [PiHole](https://hub.docker.com/r/pihole/pihole/) (DNS with built-in ad-blocking)

### Media
* [Jellyfin](https://hub.docker.com/r/linuxserver/jellyfin) (media server)
* [Radarr](https://hub.docker.com/r/linuxserver/radarr) (A movie tracker/downloader)
* [Jackett](https://hub.docker.com/r/linuxserver/jackett) (A torrent/NZB indexer)
* [Sonarr](https://hub.docker.com/r/linuxserver/sonarr) (A TV show tracker/downloader)

### Services
* [Homer](https://hub.docker.com/r/b4bz/homer) (static home page)
* [Nextcloud](https://hub.docker.com/r/linuxserver/nextcloud) (cloud platform)
* [QBitTorrent](https://hub.docker.com/r/linuxserver/qbittorrent) (Docker container running web client for QBitTorrent)

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
Set up Nginx, Jellyfin, Homer, Media Downloads Bundle, and Nextcloud:
```
#docker-compose 
#Do for all services: [nginx, jellyfin, homer, nextcloud, media-downloads]
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
* #### HTOP (Resource Monitoring): 
```
sudo apt install htop
```
* #### NeoFetch (System Information): 
```
sudo apt install neofetch
```
* #### OhMyZsh (z-shell):
```
sudo apt install szh && sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
