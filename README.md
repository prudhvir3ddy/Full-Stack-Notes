
# DigitalOcean Notes

-  create ssh key and paste ssh public key in the dashboard 
 ```bash
1. cd ~/.ssh and ssh-keygen
key is created
2. ssh root@IP_ADDRESS
fails - because we didn't mention which private key to use.
3. ssh -i ~/.ssh/private_key root@IP_ADDRESS
works but we don't have to do this everytime. 
4. vi ~.ssh/config
Host * 
	AddKeysToAgent yes
	UseKeyChain yes
5. ssh-add -K ~/.ssh/fsfe
key got added to the chain now you can do
6. ssh root@IP_ADDRESS
connected.
```
- Buying a domain
- Map domain to IP
```
1. Goto digital ocean dashboard and enter your domain in networking tab
create A record with www.domain to ip
create A record with domain to ip
map domain to nameservers

2. Goto domain dashboard, go with custom DNS and enter digital ocean nameservers
```

- server setup - create new user, disable root user
```bash
apt update
apt upgrade
adduser $USERNAME 
usermod -aG sudo $USERNAME - to give root access to user
su $USERNAME - to switch user

cat /var/log/auth.log - to check the log of users
tail -f /var/log/auth.log - to monitor the file output

cd ~
mkdir -p ~/.ssh
vi ~/.ssh/authorized_keys

and add your public ssh key there, it's important that you are adding as user and not as root

exit
exit

ssh $USERNAME@IP_ADDRESS

you should be logged in

$ chmod 644 ~/.ssh/authorized_keys
$ sudo vi /etc/ssh/sshd_config
turn off permitRootLogin

$ sudo service sshd restart
```
