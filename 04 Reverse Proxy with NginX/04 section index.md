## Reverse Proxy with Nginx

A reverse proxy is a server that sits in front of web servers and forwards client requests or traffic  (e.g. using a web browser) to those web servers. In the case of the Servpub project Jean is the server that is responsible for routing traffic to our collection of servers hosted on raspberry pis. Web Traffic comes to Jean from the public internet through a Static IP address. This traffic is then redirected by a reverse proxy server through the VPN  (using tinc). For example, a client request for wiki4print.servpub.net that is made to the Jean server will be routed to the private IP address (within the VPN) of the raspberry pi that hosts wiki4print. 

We need to configure a reverse proxy server to do that routing within our VPN.  We used Nginx for our reverse proxy server. We have used Nginx for a static server on one of our Raspberry pis (you could also use any server software of your choice, for example wiki4print uses [Mediawiki](https://www.mediawiki.org/wiki/MediaWiki) and a python flask application using [wiki-to-print from Creative Crowds](https://cc.vvvvvvaria.org/wiki/Wiki-to-print). 

Apache and NginX seem to be the two main competitors for web server software. There main pros and cons are outlined in [here](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The main difference between NGINX,to have generally better performance) and [here](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The main difference between NGINX,to have generally better performance). The gist is that NginX seems to be "better" in terms of performance and speed, it is faster because it does less out of the box.

Below is an overview of information needed for this section:
- [[02.1 NginX]] 
	- To set up a reverse proxy on the VPN server which is accessible via a static IP (for Jean in the case of Servpub), you will need to install Nginx before proceeding, further info can be found [here](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/)
- [[04.1 NginX for Reverse Proxy]]
	- This section will take you through the configuration up of a reverse proxy server which will manage traffic to other servers available on the VPN. You can read more about reverse proxy configuration with Nginx [here](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/), the instructions are taken from there. 
- [[04.2 Managing certificates for https]]
	- This section will step you through using certbot to enable the use of https which is something that needs to be configured in the Nginx config for the reverse proxy. You can find more information about https in [[00.1 Network Terminology]]

