
# DigitalOcean Notes

https://docs.google.com/presentation/d/1Mvf_rOFz1wZeH1irajJqhRQgzid7BkqJBd8wigpz39M/edit#slide=id.g616bd81ef8_0_58
Thanks to Jem Young from frontend masters

-  create ssh key in your local system and paste ssh public key in the dashboard 
 ```bash
$ cd ~/.ssh and ssh-keygen
$ key is created
$ ssh root@IP_ADDRESS
fails - because we didn't mention which private key to use.
$ ssh -i ~/.ssh/private_key root@IP_ADDRESS
works but we don't have to do this everytime. 
$ vi ~.ssh/config
Host * 
	AddKeysToAgent yes
	UseKeyChain yes
$ ssh-add -K ~/.ssh/fsfe
key got added to the chain now you can do
$ ssh root@IP_ADDRESS
connected.
```
- Buying a domain
- Map domain to IP
```
7. Goto digital ocean dashboard and enter your domain in networking tab

note: 
Maps name to IP address - A Record
Maps name to name - CNAME 
blog.prudhvi.me -> CNAME prudhvi.me
prudhvi.me -> A ip_address

ToDo: 
create A record with www.domain to ip
create A record with domain to ip

map domain to nameservers, example : ns1.digitalocean.com

9. Goto domain dashboard, go with custom DNS and enter digital ocean nameservers
```

- server setup - create new user, disable root user
```bash
# apt update
# apt upgrade
# adduser $USERNAME 
# usermod -aG sudo $USERNAME - to give root access to user
# su $USERNAME - to switch user

$ cat /var/log/auth.log - to check the log of users
$ tail -f /var/log/auth.log - to monitor the file output

$ cd ~
$ mkdir -p ~/.ssh
$ vi ~/.ssh/authorized_keys

and add your public ssh key there, it's important that you are adding as user and not as root

$ exit - to exit from user
$ exit = to exit from root

$ ssh $USERNAME@IP_ADDRESS

you should be logged in

$ chmod 644 ~/.ssh/authorized_keys
$ sudo vi /etc/ssh/sshd_config
turn off permitRootLogin

$ sudo service sshd restart
```

- nginx ( engine - x ) -web server
```bash
routes the requests to the right place
$ sudo apt install nginx
$ sudo service nginx start

now goto your $IP_ADDRESS in browser you will see nginx page

nginx configuration
$ sudo less /etc/nginx/sites-available/default
```
- Node.js  - Application server
```bash 
$ sudo apt install nodejs npm
$ sudo apt install git
```
- Application 
```bash 
$ sudo chown -R $USER:$USER /var/www
$ mkdir /var/www/app
$ cd /var/www/app && git init

$ mkdir -p ui/js ui/html ui/css
$ touch app.js
$ npm init

build the node basic app and $IP_ADDRESS:3000 should give the response

but we want entering $IP_ADDRESSS in browser should show the application response

$ sudo vi /etc/nginx/sites-available/default
location / {
	proxy_pass http://127.0.0.1:3000/;
}
```
- what about when we restart server ? we want services to be run back again.

```bash
process managers
$ sudo npm i -g pm2
$ pm2 start app.js
$ pm2 save
$ pm2 startup
```
- nginx redirect
```
location /help {
return 301 https://developer.mozilla.org/en-US/;
}
```
- nginx subdomain 
```bash 
TODO

```

- nginx file compression
```bash
vi /etc/nginx/nginx.conf

find the Gzip settings in that file

```

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
