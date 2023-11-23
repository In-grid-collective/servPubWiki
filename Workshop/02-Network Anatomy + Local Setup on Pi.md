---
theme: moon
---

<style>

    
	.tiny-font{
		font-size: 0.5em;
	}

.markdown-preview-view code{
         font-size: 0.5em;
}

</style>




## Network Anatomy 

---


## Intro




---
physical & data link layer


---
public network layer


---
virtual private network


---
server software with Nginx


---
traffic 


---


## Local Server on the Pi

---
Local vs. Remote


---
SSH

To access the pi we will use ssh [[01-Set up Pi for local Network Access#Security via SSH Keys]].

---
Terminal



---

# NGINX


We will be using [NginX](https://www.nginx.com/)  which is a web serving software. You can use Nginx to create various types of servers. We have created a simple static server using Nginx on our Pi. 

- We have installed Nginx
- And configured a simple static server
- It is serving up an index.html

---

We can change directory to where those files are being served from:

```shell
cd /var/www
```




All Nginx configuration files should be created here:

```shell
cd /etc/nginx/sites-enabled
```

Here you will see that you have a configuration for the default server, you can open that up to look at it by running:

```
nano default
```

Edit we can edit our config using nano:

```shell
nano wiki2print.conf
```


The server block is where we define our server set up:

```nginx

server {
		# 80 the default port for listening for http traffic
        listen 80 default_server;
        listen [::]:80 default_server;

        # the root folder that your website will be served out of
        root /var/www/yoursite;

		# Allowed file formats for the index / home page
        index index.html index.htm;

		# The server name that you would set
        server_name yoursite.local;

		# A location directive for any incoming URI requests
        location / {

                # First attempt to serve request as file, then as directory,
                # then fall back to displaying a 404 not found error
                try_files $uri $uri/ =404;
        }
        
 }
```



##### Create your website

We are now going to make a simple index.html page at the path that you specified in the .conf file, e.g. `/var/www/yoursite` from above.

```shell
cd /var/www 
```


Next make a new directory to match the name of your site that you set as the root path:


```shell
sudo mkdir <yoursite>
```

Change directory into that new folder:

```shell
cd <yoursite>
```

Now make your index.html using the touch command:

```shell
sudo touch index.html
```

Open it up to edit in your command line text editor of choice. We will use nano:


```shell
sudo nano index.html
```


Feel free to copy this starter index.html page for a quick test:

```html
<!DOCTYPE html>  
<html>  
<head>  
  <title>Your Site Title</title>  
</head>  
<body>  
  
	<p>
		The content of your website.
	</p>
  
</body>  
</html>
```


Restart nginx using the nginx commands:

```shell
nginx -s reload
```

To see the nginx options you can always do the help command:

```shell
nginx -h
```

Test that your site is up and available by going to the IP of your pi in your web browser! You can now edit the html file with new content to update what content is being served by your Nginx web server.


#### Using Systemctl for Nginx

We will be using [systemd](https://en.wikipedia.org/wiki/Systemd) the linux system and service manager. We can use systemctl to manage systemd services. 

Check status of Nginx:

```shell
sudo systemctl status nginx
```

Reload any Nginx configuration files that have changed (in sites-enabled). This is a safer process as it does not shut down the Nginx process entirely:

```shell
sudo systemctl reload nginx
```
