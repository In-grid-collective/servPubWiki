
The next step is to setup the software needed to make a set up a server on your pi. You can use a number of different tools and programming languages to create servers.

We will be using [NginX](https://www.nginx.com/)  which is a web serving software. You can use Nginx to create various types of servers. We will be creating a simple static server using Nginx on our Pi. We have also used Nginx for our reverse proxy server that redirects traffic coming in from the public internet, which will come to later here [[05-VPN and Reverse Proxy Server.md#NginX Reverse Proxy Server]]

Back to the server set up on our pi! 

# NGINX

We will put nginx on our pi! To do this we will need to be working on our pi. To enter the pi we will use ssh [[01-Set up Pi for local Network Access.md#Security via SSH Keys]].

Once in the pi, we will install the open source nginx from the command line using apt, there are [many other ways to install](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/). apt is a tool which allows you to download and install packages onto your device. 

``` shell
apt install nginx
```


Note: packages are saved by default in your user's home directory. 

If you went to the IP address of your pi in web browser on a device that was on the same wifi network as the pi you would be able to see the default Nginx site already! Where is that coming from?

Well, Nginx will create some default html files on installation, to find that HTML index page in your file system on the pi, change directory :

```shell
cd /var/www/html 
```


The default html document will be named `index.nginx-debian.html` which you can see if you run the `ls` command now. 

The default configuration file for this simple static site can be found here:

```shell
cd /etc/nginx/sites-enabled
```

Here you will see that you have a configuration for the default server, you can open that up to look at it by running:

```
nano default
```

Here is shortened version of what you should see with some added comments:

```nginx

server {
		# 80 the default port for listening for http traffic
        listen 80 default_server;
        listen [::]:80 default_server;

        # the root folder that your website will be served out of
        root /var/www/html;

		# Allowed file formats for the index / home page
        index index.html index.htm index.nginx-debian.html;

		# The server name that you would set
        server_name _;

		# A location directive for any incoming URI requests
        location / {

                # First attempt to serve request as file, then as directory,
                # then fall back to displaying a 404 not found error
                try_files $uri $uri/ =404;
        }
        
 }
```

This server block is where we define our server set up, this [nginx beginners guide does a good job of introducing the fundamentals](https://nginx.org/en/docs/beginners_guide.html). 


IMPORTANT: __DO NOT__ simply edit that default configuration file. Instead set up your own folder within `sites-enabled` to start serving your custom static site. This [Pitfalls and Common Mistakes guide](https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/) should serve as a good resource to double check any changes you make. 


# Customising an NGINX Site

We are going to create a custom .conf file and point it to a new location on your pi for your custom site. 

##### Create a custom .conf for you site

We will make a copy of the default file and rename that copy, and then move the default configuration to `sites-available` which is where you can keep any configuration files for sites that are not currently active. 

Make sure you are in `sites-enabled` (see above):

```shell
cp default <yoursite>.conf
```


Edit your config:

```shell
nano <yoursite>.conf
```


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



You can now remove the default config:

```shell
rm default
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

Stop Nginx:

```shell
sudo systemctl stop nginx
```

Start Nginx:

```shell
sudo systemctl start nginx
```

Reload any Nginx configuration files that have changed (in sites-enabled). This is a safer process as it does not shut down the Nginx process entirely:

```shell
sudo systemctl reload nginx
```

Force restart Nginx:

```shell
sudo systemctl restart nginx
```



