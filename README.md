# Cloud hosting Notes

This short blog will provide you each detail to get started with cloud hosting that preferably works fine with most of the **Ubuntu** server providers out there in the market. The commands written in this would work absolutely fine with Linux/Mac.

> Thanks to [**Jem Young**](https://docs.google.com/presentation/d/1Mvf_rOFz1wZeH1irajJqhRQgzid7BkqJBd8wigpz39M/edit#slide=id.g616bd81ef8_0_58), Frontend masters

## Contents

1. [Creating and storing SSH Keys](ssh_config.md)
2. [Buying a domain and mapping it to our server](domain_mapping.md)
3. [Server setup](server_setup.md)
4. [MongoDB setup](mongo_setup.md)
5. [Nginx setup](nginx_setup.md)
6. [Redis setup](redis_setup.md)

- security

```bash
$  sudo apt install unattended-upgrades
$  cat /etc/apt/apt.conf.d/50unattended-upgrades

security checklist:
	- ssh
	- firewalls
	- unattended-upgrades
	- two factor authentication
	- VPN

nmap:
$ sudo apt install nmap
$ nmap YOUR_SERVER_IP_ADDRESS
$ nmap -sV YOUR_SERVER_IP_ADDRESS

try to close that you don't need, open port means it's exposed to internet and someone gonna find if any vulnerabilities show up

$ less /etc/services - to see all the ports running

Firewall :
- iptables -p tcp --dport 80 -j REJECT
- UFW (uncomplicated firewall) is easy tool to do firewall stuff

$ sudo ufw status
$ sudo ufw enable
$ sudo ufw allow ssh

test:
try disabling http so we cannot see our homepage in browser

$ sudo ufw reject http

```

- file permissions

```bash
TODO
```

- Adding https to digital ocean ubuntu server

```

use certbot
route your traffic to https (domain name needed for sure)
https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-20-04
```

- Adding http/2 to server

```
http2 needs https enabled

$ sudo vi /etc/nginx/sites-available/default
listen 443 http2 ssl;

```

- Websocket

```
location / {
   proxy_set_header Upgrade $http_upgrade;
   proxy_set_header Connection "upgrade";

   proxy_pass http://127.0.0.1:3000;
}

```

- nginx subdomain

```bash
create A record in digitalocean dashboard with subdomain.domain.com

create folder in /var/www/subdomain.domain.com/ and keep your files there

in nginx

create file in /etc/nginx/sites-available/subdomain.domain.com

and write config there

server {

        root /var/www/html;
        index index.html index.htm index.nginx-debian.html;

        server_name blog.prudhvireddy.me www.blog.prudhvireddy.me;

        location / {
                try_files $uri $uri/ =404;
        }
}

link two files

$ sudo ln -s /etc/nginx/sites-available/subdomain.domain.com /etc/nginx/sites-enabled/subdomain.domain.com

add it to https

sudo certbot --nginx -d blog.prudhvireddy.me -d www.prudhvireddy.me

```
