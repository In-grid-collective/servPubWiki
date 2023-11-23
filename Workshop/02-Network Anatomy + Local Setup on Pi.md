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


## Break

Download GitBash

---

## Network Anatomy 

---


## Intro




---

![[slide1Asset 1.png]]

---
![[slideAsset 2.png]]

---
![[slideAsset 3.png]]

---

![[slideAsset 4.png]]

---


![[slideAsset 5.png]]
---

## Local Server on the Pi

---

## Local vs. Remote

---

## Set up Pi for Local Network Access


SSH or Secure Shell is a network communication protocol that enables two computers to communicate. We will use SSH to access the Pi remotely via the local network

![[Pasted image 20231108164245.png]]
*Image courtesy of Mara, Systerserver*

--
## Prerequisites

- Raspberry pi + peripherals: HDMI screen, keyboard, mouse etc. 
- Pi OS booted: The Rosa Server Guide recommends [Armbian OS](https://www.armbian.com/rpi4b/)
- Knowledge of terminal/bash
- Have SSH installed on laptop (most OS have it by default now, if not then manually install, below)
- To be on the same local network

--

## Notes on terms

Server = the Pi

Client = your laptop

--

## So far

We have got a Pi setup with:
- Armbian OS

We prepared this ahead of today in the interest of time. This is a process of downloading and installing the Armbian, which is a Linux operating system.

Full instructions live on our docs.

--

## Installing SSH server & client

The next step is to install an SSH server to the server (Pi) and a client package to the client (your laptop).

To install the server package to the Pi:

``` shell
sudo apt install -y openssh-server
```

To install the server package to your laptop:

``` shell
sudo apt install -y openssh-client
```

--

## Connecting from a client

The first thing we need to do is find out the IP address of the device we want to access - the Pi. One way to do this is, through is this command line on the pi:

```shell
hostname -I
```

We have already done this, and added the Pi address for you here on the Pad: https://digitalcare.noho.st/pad/p/serv.pub_wrk.shp

--

Now that we know the IP address of the Pi, we can SSH into it, using the following command:

``` shell
ssh pi@<pi ip>
```

eg.

``` shell
ssh pi@100.10.1.90
```

--
You are now logging into the Pi, so you'll be asked for a password. 

--

Once you've successfully entered the PW, you should see your terminal username change from your device's local user to something like:


``` shell
pi@raspberry~$.
```


--

## We're in!

--

To exit the Pi (not now!) use the following command:

``` shell
exit
```


---
## Tmux

We are now going to join in on a shared terminal session. To do this we will be using a terminal multiplexer called Tmux. We have already installed this on the Pi - again, fuller instructions exist in our docs.

![[Pasted image 20231123170353.png]]


--
To access a shared session, we all have to be logged in as the same user simultaneously. The tmux session will be registered under that user - in our case we want to create a session as `sudo`.

`sudo` stands for [Super User DO](https://www.geeksforgeeks.org/sudo-command-in-linux-with-examples/). This command is used to allow you to run commands that only super users are allowed to run, and is often followed by another command you want to run. When you use `sudo` you will be prompted to enter your super user password to execute the command:

```shell
 sudo <command>
```

--

If you want to enter into sudo mode to run all commands as a super user (we do), run:

```shell
 sudo su
```

This will then prompt you to enter your password and after which you can run commands as a super user.

--

We're now going to start our tmux session with the following command:

```shell 
tmux attach
```

--
Note: Everyone in a Tmux session is acting as the same user. However we can create split screens and multiple panes within Tmux so different people can work on different things. 

Reference and troubleshooting: https://www.howtogeek.com/devops/how-to-get-started-and-use-tmux/

---
Terminal



---
Tmux

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
