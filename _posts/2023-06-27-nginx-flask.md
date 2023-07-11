---
layout: post
title: "Flask + Nginx + Waitress"
date:   2023-06-27 12:00:00 -0800
categories: web
---

I've deployed several web sites to hosting services over the years but it's been a while since I've deployed them locally. New technologies have come about and I wanted to make sure I don't get left behind. I picked NGINX and Waitress based on reviews praising their ease-of-use and versatility. 

The web application needs to run as a service that Nginx will send traffic to. That's one of the key things to remember. The application doesn't run inside Nginx. Makes the flow of traffic easier to visualize. A request gets to the server on port 80. The request gets redirected to Nginx. Based on filters such as the URL, it gets directed to the service. The service generates a response and sends it back. This is how a single web server can host multiple web sites. Similar to NAT.

## enable systemd on ubuntu wsl

`sudo vi /etc/wsl.conf`


### wsl.conf contents
```
[boot]
systemd=true
```


## create service that starts the waitress server

`sudo vi /etc/systemd/system/<application name>.service`

### contents of .service file

```
[Unit]
Description=<application description>
After=network.target

[Service]
User=<username>
WorkingDirectory=<app location>
ExecStart=<waitress-serve command>
Restart=always

[Install]
WantedBy=multi-user.target
```

```
[Unit]
Description=Car Maintenance Flask application
After=network.target

[Service]
User=carlo
WorkingDirectory=/home/carlo/projects/car-maintenance
ExecStart=/home/carlo/projects/car-maintenance/.venv/bin/waitress-serve --host 127.0.0.1 --port 8000 app:app
Restart=always

[Install]
WantedBy=multi-user.target
```

if app is stored in a variable
`waitress-serve --host 127.0.0.1 hello:app`

if app is returned by a function
`waitress-serve --host 127.0.0.1 --call hello:create_app`

### start the service

`sudo systemctl start <application name>.service`

if you want it to start automatically

`sudo systemctl enable <application name>.service`

## configure nginx

`sudo vi /etc/nginx/sites-enabled/default`

### contents of config file
```
server {
	listen 80 default_server; 
	listen [::]:80 default_server;

	server_name _;
	
	location /static/ {
		alias /home/carlo/projects/car-maintenance/static/;
	}

	location / {
		proxy_pass http://localhost:8000; 
		include /etc/nginx/proxy_params;
		proxy_redirect off; 
		proxy_set_header X-Real-IP $remote_addr;
	}
}
```

## Add the nginx user to the group that owns the static files

`sudo usermod -a -G carlo www-data`


## Restart nginx and the application service

`sudo nginx -s reload`

`sudo systemctl restart <application name>.service`


