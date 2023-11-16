
The next step is to setup the software needed to make a set up a server on your pi. You can use a number of different tools and programming languages to create servers.

We will be using [NginX](https://www.nginx.com/)  which is a web serving software. You can use Nginx to create various types of servers. We will be creating a simple static server using Nginx on our Pi. We have also used Nginx for our reverse proxy server that redirects traffic coming in from the public internet, which will come to later here [[05-VPN and Reverse Proxy Server#NginX Reverse Proxy Server]]

Back to the server set up on our pi! 

# NGINX

We will put nginx on our pi! To do this we will need to be working on our pi. To enter the pi we will use ssh [[01-Set up Pi for local Network Access#Security via SSH Keys]].

Once in the pi, we will install the open source nginx from the command line using apt, there are [many other ways to install](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/). apt is a tool which allows you to download and install packages onto your device. 

``` shell
apt install nginx
```


Note: packages are saved by default in your user's home directory. 

Nginx will create some default html files on installation, to find that HTML index page go to: 

```shell
cd /var/www/html 
```


The default html document will be named `index.nginx-debian.html`

The default configuration file for this simple static site can be found here:

```shell
cd /etc/nginx/sites-enabled
```

Here you will see that you have a configuration for the default server, you can open that up to look at it by running:

```
nano default
```

Here is shortened version of what you should see with some added comments:

```

server {
		# 80 the default port for listening for http traffic
        listen 80 default_server;
        listen [::]:80 default_server;

        # the root folder that your website will be served out of
        root /var/www/html;

		# Allowed file formats for the index / home page
        index index.html index.htm index.nginx-debian.html;

		# The server name that you would set
        server_name servername;

		# A location directive for any incoming URI requests
        location / {

                # First attempt to serve request as file, then as directory,
                # then fall back to displaying a 404 not found error
                try_files $uri $uri/ =404;
        }
        
 }
```

This server block is where we define our server set up, this [nginx beginners guide does a good job of introducing the fundamentals](https://nginx.org/en/docs/beginners_guide.html). 


# Customising an NGINX Static Web Site


IMPORTANT: __DO NOT__ simply edit that default configuration file. Instead set up your own folder within `sites-enabled` to start serving your custom static site. This [Pitfalls and Common Mistakes guide](https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/) should serve as a good resource to double check any changes you make. 

We will make a copy of the default file and rename that copy, and then move the default configuration to `sites-available` which is where you can keep any configuration files for sites that are not currently active. 






Dive Deeper:

The way nginx and its modules work is determined in the nginx configuration file. By default, the configuration file is named `nginx.conf`. For nginx open source, `nginx.conf` is placed in the directory `/usr/local/nginx/conf`, `/etc/nginx`, or `/usr/local/etc/nginx` depending on your operating system.

To find your `nginx.conf` on a pi with Armbian installed [[01-Set up Pi for local Network Access#Armbian]] run this change directory command:

```shell
cd /etc/nginx
```

Here you will find your `nginx.conf` file. You can confirm that it is there by running `ls` command on your command line. For our purposes we won't be changing our `nginx.conf`



