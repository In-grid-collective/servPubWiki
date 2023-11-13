
The next step is to setup the software needed to make a set up a server on your pi. You can use a number of different tools and programming languages to create servers.

We will be using [NginX](https://www.nginx.com/)  which is a web serving software. You can use Nginx to create various types of servers. We will be creating a simple static server using Nginx on our Pi. We have also used Nginx for our reverse proxy server that redirects traffic coming in from the public internet, which will come to later here [[05-VPN and Reverse Proxy Server#NginX Reverse Proxy Server]]

Back to the server set up on our pi! 

# NginX

We will put NginX on our pi! To do this we will need to be working on our pi. To enter the pi we will use ssh [[01-Set up Pi for local Network Access#Security via SSH Keys]].

Once in the pi, we will install nginx from the command line using apt. apt is a tool which allows you to download and install packages onto your device.

``` shell
apt install nginx
```

Note: packages are saved by default in the /home directory. NginX will then create your html files, which will then become your website.

**IMPORTANT** To find the HTML index page go to: /home/var/www/html. 

The document will be named by default as "index.nginx-debian.html"

Before doing anything else, consider whether to relocate nginx and how to name the documents. We would advise you not necessarily move nginx but you may find it easier in the long run to rename the files it creates so you can more easily understand i.e. we have changed "index.nginx-debian.html" to "index.servpub.html"


# Customising a Static Nginix Site

We will step you through the Niginx configuration we used to create a simple html page at servpub.net, such as this documentation you are currently reading! You won't be able to access this on the public internet, but you will be able to access it locally in your browser.

