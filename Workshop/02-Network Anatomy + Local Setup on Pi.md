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
	
	pre{
	     font-size: 0.7em!important;
	}
	ul,
	p{
		font-size: 0.8em!important;
	}
	code{
	     padding: 0.5em 1em!important;
	}
	code.nginx{
	    font-size: 0.4em!important;
	    line-height: 1.5em!important;
	}

	table{
		 font-size: 0.5em!important;
		 margin: 40px 0!important;;
	}

</style>



## Network Anatomy 

We will start with an overview of servpub to introduce different elements of internet infrastructure and associated terms.

---


This will be followed by an introduction to the tools for working with the wiki2print pi on a local network, before later moving onto mounting the raspberry pi into the Virtual Private Network accessible online.

| __Local__ Devices in __Local__ Area Networks |  Remote Devices on the Public Internet  |
| ----------- | ----------- |
| Devices that are connected via the same wi-fi or ethernet in a limited geographical area. | Devices that are connected via the wider public internet.  |


---


![[slideAsset 6.png]]

---
![[slideAsset 2.png]]

--



| Term     | Example | Description |
| ----------- | -----------  | ----------- |
| IP Address   |  192.0.2.1 | IP (Internet Protocol) addresses work like a bit like a street address in that they allow devices to find each other online.      |
| Static IP |  89.106.208.4 | Unlike standard dynamic IPs, a static IP is assigned to a device and it remains the same. |
| Domain Name |  servpub.net | IP addresses can be converted into text via a Domain Name System (DNS) format.   |
| Ports | 80 | Port 80 is the port number assigned to internet communication protocol, Hypertext Transfer Protocol (HTTP). Ports are part of the TCP transport layer.|



---
![[slideAsset 7.png]]

--



| Term     | Example | Description |
| ----------- | -----------  | ----------- |
| Tinc |   | VPN software |
| Local IP Addresses   |  10.0.0.0| IPs in this range are reserved for private networks     |




---

![[slideAsset 4.png]]

---


![[slideAsset 5.png]]
--

| Term     |  Description |
| ----------- | ----------- |
| Static Server |  A Server for serving up files like .html .jpeg .css .js etc  |
| Proxy Server | A proxy server is a go‑between or intermediary server that forwards requests for content from multiple clients to different servers|
| Reverse Proxy Server  |  A **reverse proxy server** is a type of proxy server that typically sits behind the firewall in a private network and directs client requests to the appropriate backend server    |
| Nginx |  **Nginx** is open source software for web serving |

---

## Local Server on the Pi

--

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

---
## Break

Download GitBash

If you haven't already checked if you have ssh, try to check the ssh version:

``` shell
ssh -v
```


If you get a "command not found", install the client package to your laptop with this line on linux:

``` shell
sudo apt install -y openssh-client
```


---

## So far

We have got a Pi setup with:
- Armbian OS

We prepared this ahead of today in the interest of time. This is a process of downloading and installing Armbian, which is a Linux operating system. Full instructions live on our docs.

The next thing we need to do is access our pi via ssh, but to do that we need to understand a bit about the terminal.


---
## Terminal

Linux and Mac OS systems have Unix Shells that provide us a command line interface. e.g. Bash Shells or Zsh 

Windows uses Power Shell by default, which has a different command language. If you are on Windows you will need a Bash shell installed for the following commands. 


--

## Who are you?

Show the name of the currently logged in user:

```shell
whoami
```

Show the name of the machine is given on the network: 

```shell
hostname
```


--
## Folder Hierarchies

Much of working on the command line involves navigating around your system without the use of a Graphical User Interface (GUI). 

However, we will essentially be navigating around folder hierarchies like below:

<pre style="background-color:black; padding:20px">
root/
├─ directoryname/
       ├─ anotherdirectory/
                  ├─ aFileName.txt
</pre>


--

- __directory__ : also known as a folder
- __root directory__ :  "highest" directory in the hierarchy. You can also think of it as the start of a particular folder structure,  denoted by a tilde `~` on the command line.
- __directory path__ :  in the example above, there would be a directory called "directoryname", which has a folder called "anotherdirectory" inside of it, the path would be: `/directoryname/anotherdirectory`

--



Print working directory that you are currently in:

```shell
pwd
```


List current directory contents:

``` shell
ls
```

--

Change directory to a directory using the "path" to that folder/directory, if the path starts with `/`  it will start at the root directory:

```shell
cd /directoryname/anotherdirectory
```

Change directory to root :

```shell
cd 
```

Change directory to the directory above the one you are currently in:

```shell
cd ..
```


---

## Installing SSH 

We have installed an SSH server on the (Pi) and you will need an ssh client package (on your laptop). If you haven't already checked if you have ssh, try to check the ssh version:

``` shell
ssh -v
```

If you get a "command not found", install the client package to your laptop with this line on linux:

``` shell
sudo apt install -y openssh-client
```

--

## Connecting from a client

The first thing we need to do is find out the IP address of the device we want to access - the Pi. One way to do this is, through is this command line on the pi:

```shell
hostname -I
```

We have already done this, for those in the room we have added the Pi address for you on the Pad.

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

We will enter into sudo mode to run all commands as a super user run:

```shell
 sudo su
```

This will then prompt you to enter your password and after which you can run commands as a super user.

--

We're now going to start our tmux session with the following command:

```shell 
tmux attach
```



> [!Note]
> Everyone in a Tmux session is acting as the same user. However we can create split screens and multiple panes within Tmux so different people can work on different things. 


---

## Server Software 


We will be using [NginX](https://www.nginx.com/)  which is a web serving software. You can use Nginx to create various types of servers. We have created a simple static server using Nginx on our both pis. So Far:

- We have installed Nginx
- And configured a simple static server
- It is serving up html


--

### Nginx configuration files for servers

All Nginx configuration files should be created here:

```shell
cd /etc/nginx/sites-enabled
```

Do `ls` to list all files. We can edit our config using nano:

```shell
nano wiki2print.conf
```

--

The server block is where we define our server set up. Take note of the root folder path, that is where our website is being served out of: 

```nginx

server {
		# 80 the default port for listening for http traffic
        listen 80 default_server;
        listen [::]:80 default_server;

        # the root folder that your website will be served out of
        root /var/www/wiki2print;

		# Allowed file formats for the index / home page
        index index.html index.htm;

		# The server name that you would set
        server_name wiki2print.local;

		# A location directive for any incoming URI requests
        location / {

                # First attempt to serve request as file, then as directory,
                # then fall back to displaying a 404 not found error
                try_files $uri $uri/ =404;
        }
        
 }
```


--

### Using Systemctl for Nginx

We will be using [systemd](https://en.wikipedia.org/wiki/Systemd) the Linux system and service manager. We can use systemctl to manage systemd services. 

Check status of Nginx:

```shell
sudo systemctl status nginx
```

Reload any Nginx configuration files that have changed (in sites-enabled):

```shell
sudo systemctl reload nginx
```


--

## Our Static Website

Inside of www we will see our wiki2print folder which is serving up our html:

<pre style="background-color:black; padding:20px">
var/
├─ www/
       ├─ wiki2print/
                  ├─ index.html
</pre>

Change directory to the root:

```shell
cd /var/www/wiki2print
```


--

Print out the contents of the index.html to terminal:

```shell
cat index.html
```

Let's open it up in Nano command line text editor to make some changes:

```shell
nano index.html
```

--

### Nano Quick Start

- You will see the nano editor open. At the bottom of the window, you will find some shortcuts to use with the Nano editor. The `^` (caret) means that you must press **CTRL** (Windows) or **control** (macOS) to use the chosen command.
- Press **CTRL + O** to save the changes made in the file and continue editing.
- To exit from the editor, press **CTRL + X**. If there are changes, it will ask you whether to save them or not. Input **Y** for **Yes**, or **N** for **No**, then press **Enter**. If there are no changes, you will exit the editor right away.


Every time we save we should be able to refresh our browsers to see the change!

--

### More Terminal Commands

Make new directory:

 ```shell
mkdir directoryname
```


Make new file:

```shell
touch example.txt
```

--

Copy a text file called `example.txt` which is in a sub directory "exampleFolder" to a new location  `/path/to/folder`

```shell
cp exampleFolder/example.txt /path/to/folder
```

--

### ! Be careful with removing things !
Remove a file:

```shell
rm <file.txt>
```


Remove an empty directory:

```shell
rmdir <directoryname>
```

Remove a directory and all the items inside of it:

```shell
rm -r <directoryname>
```


