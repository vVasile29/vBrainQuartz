## Hardware

- Raspberry Pi 4 Model B 8gb
- heat sinks
- 15 W AC Adapter
- M2 NVME Adapter + 1TB M2 NVME
- SD Card

## OS

1. download `rpi-imager` from https://downloads.raspberrypi.org/imager/imager_latest_amd64.deb
2. install via `sudo dpkg -i imager_X.Y.Z_amd64.deb`
3. insert SD-Card/SSD and make setup ![[Pasted image 20240915133843.png]]
	1. set PI 4 Model B, Raspberry PI OS and Storage (Pi is initially set to boot from SD Card, I made initial setup via SD card, then booted and changed boot order, then repeated process for SSD) - *there must be an image that automatically starts with boot order USB...*
	2. edit details (pub SSH key from PC you want to reach the Pi from), LAN SSID + PW, locale, hostname and username + PW

## Change boot order

1. Update
 ```zsh
 sudo apt update   
 sudo apt full-upgrade   
 sudo reboot
 ```
2. check current running version of bootloader EEPROM image
```zsh
vcgencmd bootloader_version
sudo rpi-eeprom-update
```
3. upgrade to latest stable
```zsh
sudo nano /etc/default/rpi-eeprom-update  
```
	Change the word "critical" or "default" to "stable" and save the file
```zsh
sudo rpi-eeprom-update
```
4. apply the update and reboot
```zsh
sudo rpi-eeprom-update -a   
sudo reboot
```
5. check BOOT ORDER config
```zsh
sudo rpi-eeprom-config
```
	The default code is **0xf41** read right to left to determine the boot order.
	**1 = Check SD card**  
	**4 = Check USB drive**  
	**f = Start again**
	-> we will set **0xf14** to prioritize USB
```zsh
sudo -E rpi-eeprom-config --edit
```
	add BOOT_ORDER=0xf14

## Host first website (with nginx and express.js webapp)

0. (install zsh)
```zsh
sudo apt install zsh
chsh -s /usr/bin/zsh
```
	log out

```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
	also add this to .zshrc so nvm will be found

```zsh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

1. install node and npm 
```zsh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
nvm install 20
node -v # should print `v20.17.0`
npm -v # should print `10.8.2`
```
2. go inside directory and `npm init -y`
3. `npm install express`
4. create `server.js` and empty `count.txt`

```js
const { readFileSync, writeFileSync } = require('fs');

const express = require('express');
const app = express();

app.get('/', (req, res) => {
  const count = readFileSync('./count.txt', 'utf-8');
  console.log('count ', count);

  const newCount = parseInt(count) + 1;
  writeFileSync('./count.txt', newCount.toString());

  res.send(`
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1" />
      <title>RPi Hosted Website</title>
    </head>
    <body>
      <h1>Welcome to my Website!</h1>
    <p>This page has been viewed ${newCount} times!</p>
  </body>
</html>
  `);
});

app.listen(5000, () => console.log('http://localhost:5000/'));
```
5. run it via `node server.js`
6. install nginx via `sudo apt install nginx`
7. add proxy to webapp which is running on port 5000 via `/etc/nginx/sites-available`
```
location / {
proxy_pass http://localhost:5000;
...
}
```
8. apply changes via `sudo nginx restart`

## Docker

new docker compose and dockerfile are located in `/opt/stacks`

### Install Docker

```zsh
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
### Dockerize application

docker packages software so it can run on any hardware

1. dockerfile is a blueprint for creating images
2. images is a template for a container
3. container is a running instance of an image

`docker ps` shows all the running containers on the system, `-a` shows past ones aswell

#### Dockerfile

```Dockerfile
# base image, take one that has node installed
FROM node:12

# add app source code to the image
WORKDIR /app

COPY package*.json ./

# (shell form)
RUN npm install

# we want to copy everything but the node_modules, as that gets installed in the layer before, so it's defined in the .dockerignore
COPY . .

# set environment variable for port inside the continainer, which can be used by the javascript for instance
ENV PORT=8080

# access node.js app publicly
EXPOSE 8080

# (exec form - doesn't start up a shell session)
CMD [ "npm", "start" ]
```

#### Build Docker Image

`sudo docker build -t vasile29/my-first-docker-image:1.0 .` (path to docker file, just a `.` as it was in the current directory)

#### Run Docker Container

`sudo docker run -p 5000:8080 dockerimageid` (local:container), this implements port forwarding from docker container to the local machine (or remote server, pi in our case, as we ssh onto it)

## Port forwarding to access website over internet

![[Pasted image 20240915165626.png]]

## Nginx Proxy Manager

- manages open ports 80 (HTTP) and 443 (HTTPS), because otherwise for every website/service we would have to open an new port. This way, all we need to open up is 2 ports
- nginx already running on port 80 on default, so stopped service with `sudo systemctl stop nginx`
- started nxinx proxy manager with docker-compose.yml

```yml
version: '3.8'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```

- ran it with `sudo docker compose up -d`

port 81 also forwarded to see the admin panel from vPC, it would be on localhost in pi@pi.local
-> better to do ssh local port forwarding `ssh -L 8081:localhost:81 pi@pi.local` (local:localhost:port)

also add proxy host per application (port) + SSL certificates (select request new SSL certificate)
![[Pasted image 20240916010139.png]]

==Router <-> vPC:5000 <-> PI:5000 <-> Docker:8080==

## Dynamic DNS

create account at no-ip.com 
![[Pasted image 20240916005841.png]]

with this, now set cname in doman provider 
![[Pasted image 20240916005931.png]]

and add the account to speedport ip router for dynamic ip updates

> [!NOTE]
> → this will be repeated for all new docker containers and will be accessible via `<container>.vasile.digital`:
> 
> - https://pi.vasile.digital/
> - https://games.vasile.digital/
> - https://portainer.vasile.digital/
> - https://npm.vasile.digital/

## Portainer

1. `sudo docker pull portainer/portainer-ce:latest`
2. `sudo docker run -d -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest`

## Pihole #todo

https://pimylifeup.com/pi-hole-docker/

## Jellyfin #todo

https://pimylifeup.com/jellyfin-docker/

...