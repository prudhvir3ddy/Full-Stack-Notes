# nginx ( engine - x )

###### a web server and load balancer

1. Installing **Nginx**
   routes the requests to the right place

```bash
$ sudo apt install nginx
$ sudo service nginx start
```

2. Now goto your $IP_ADDRESS in browser you will see nginx page
   nginx configuration

```bash
$ sudo less /etc/nginx/sites-available/default
```

### Deploying a Node.js app

1. Installing Node.js & npm

```bash
$ sudo apt install nodejs npm
$ sudo apt install git
```

2. Create a sample Application

Add required permissions to the current user

```bash
$ sudo chown -R $USER:$USER /var/www
```

3. Create a new directory for your app

```bash
$ mkdir /var/www/app
$ cd /var/www/app && git init

$ mkdir -p ui/js ui/html ui/css
$ touch app.js
$ npm init
```

4. Build the node basic app and `$IP_ADDRESS:3000` should give the response,but we want us just by entering `$IP_ADDRESSS` in browser should show the application response. So we need to do reverse proxying for that

```bash
$ sudo vi /etc/nginx/sites-available/default
```

Add the following tag in that config file

```bash
location / {
	proxy_pass http://127.0.0.1:3000/;
}
```

5. Restart the server

```bash
$ sudo systemctl restart nginx
```

6. What about when we restart server ? We want services to be run back again. We can avoid this by using `process managers`

```bash
$ sudo npm i -g pm2
$ pm2 start app.js
$ pm2 save
$ pm2 startup
```

7. Nginx redirect

```bash
location /help {
  return 301 https://developer.mozilla.org/en-US/;
}
```

8. Nginx file compression

```bash
vi /etc/nginx/nginx.conf

find the Gzip settings in that file

```
