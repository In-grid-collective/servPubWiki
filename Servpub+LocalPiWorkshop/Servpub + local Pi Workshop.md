---
theme: moon
---
<header>
<link rel="stylesheet" media="screen" href="https://fontlibrary.org//face/generale-station" type="text/css"/>
</header>

<style>

	code.nginx{ 
		font-size: 0.4em!important; 
		line-height: 1.5em!important; 
	} 
	table{ 
		font-size: 0.5em!important; 
		margin: 40px 0!important;
		color:white!important;
	}

    
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
		color:white!important;
	}
	h1,h2,h3{
        font-family : "GeneraleStationRegular"!important;
        font-size: 1.5em!important;
}
	code{
	     padding: 0.5em 1em!important;
	}

.slide-background{
background: rgb(79,9,121); background: linear-gradient(180deg, rgba(79,9,121,1) 4%, rgba(52,1,45,1) 100%)!important;

}

</style>


<!-- .slide: class="bg" -->
![[ServpubTerrain.png]]

---
## ServPub 
...is a platform for research and practice on experimental decentralised computational publishing, to reflect collectively on affective infrastructures, minor tech and autonomous networks within, and beyond, institutional constraints.

ServPub is run by artists, coders, activists, collectives, scholars and researchers using FLOSS, who share feminist values and practices. We aim to build shared knowledge and resources which operate at small scale and as part of grassroots community networks to explore alternatives. 


---
# Collaborators

<br/>
#### Organising bodies: 
Winnie Soon, [Slade School of Fine Art](https://www.ucl.ac.uk/slade/) (previously at CCI)
<br/>
Geoff Cox, [CSNI](https://www.centreforthestudyof.net/?page_id=756) at London South Bank Uni.
<br/>
Christian Ulrik Andersen, [DARC](https://darc.au.dk) & [Shape](https://shape.au.dk) at Aarhus Uni

---
#### Participating Collectives: 
[In-Grid](https://www.in-grid.io/) Project Implementation w/
<br/>
[Systerserver](https://systerserver.net) Mara and Ooooo: Infrastructure/VPN 
<br/>
[Varia](https://cc.vvvvvvaria.org) Luke: First workshop guide - local setup 
<br/>
[CC](https://cc.vvvvvvaria.org/) Manetta and Simon: wiki-to-print 

---

![[ServpubTerrain.png]]
[servpub.net](https://servpub.net)  |  [wiki4print.servpub.net](https://wiki4print.servpub.net/)

---
# In-Grid 


Batool / Becky / George / Katie / Sunni / Johanna

+
![[Batool.png|110]] ![[Becky.png|110]]![[George.png|110]]![[Katie.png|110]]![[Sunni.png|110]] ![[Johanna.png|110]]
 
_In-grid is a group of many - these are the members who are actively involved in this project_

---

# Resources:
| _From CCI_                      | _From In-grid_        | _From Systerserver_                                                       |
| ------------------------------- | --------------------- | ------------------------------------------------------------------------- |
| x2 raspberry pies & peripherals | Screens & peripherals | Public IP / Server (Jean)                                                 |
| SD cards & 4G dongle            | Space + electricity   | Rosa Manual - (Varia, HYPHA, LURK, esc, Feminist Hack Meetings, Constant) |


---
# Resources:

## Time!
A resource we all had to share, split and make


---

## Timeline

<br/>

- May '23 - Public workshop 1 (CCI), project launch
- August '23 - Semi-public workshop -  tinc & nginx
- November '23  - Public workshop (LSBU) wiki node joins VPN
- December '23 - wiki-to-print becomes wiki4print (installation)
- Jan '24 - [[[https://transmediale.de/en/2024/event/research-workshop-2024](https://transmediale.de/en/2024/event/research-workshop-2024|Transmediale workshop]]) and [[[https://darc.au.dk/publications/peer-reviewed-newspaper](https://darc.au.dk/publications/peer-reviewed-newspaper|content/form newspaper]])
- April '24 - Start of the [Servpub Experimental Book Project](https://copim.pubpub.org/pub/servpub-book-launch/release/1)

<br/>

_Between public facing events there were many meetings, co-working sessions, discussions and collective debugging._

---

## Network Anatomy 

Here is an overview of servpub to introduce different elements of internet infrastructure and associated terms.


---



![[slideAsset 6.png]]

---

![[piBerlin.jpeg]]

---

![[slideAsset 2.png]]


---



| Term     | Example | Description |
| ----------- | -----------  | ----------- |
| IP Address   |  192.0.2.1 | IP (Internet Protocol) addresses work a bit like a street address in that they allow devices to find each other on a network.      |
| Static IP |  89.106.208.4 | Unlike standard dynamic IPs, a static IP is assigned to a device and it remains the same. |
| Domain Name |  servpub.net | IP addresses can be mapped to a name/text via a Domain Name System (DNS) format.   |
| Ports | 80 | Port 80 is the default port number assigned to internet communication protocol, Hypertext Transfer Protocol (HTTP). Ports are part of the TCP transport layer.|



---

![[slideAsset 7.png]]

---



| Term | Example | Description |
| ---- | ---- | ---- |
| Tinc |  | VPN software |
| Subnet IP Addresses | 10.10.12.* | IPs in this range are reserved for private networks |


---


![[ifconfigberlin.jpeg]]


---

![[slideAsset 4.png]]

---


![[slideAsset 5.png]]

---


| Term     |  Description |
| ----------- | ----------- |
| Static Server |  A Server for serving up files like .html .jpeg .css .js etc  |
| Proxy Server | A proxy server is a go‑between or intermediary server that forwards requests for content from multiple clients to different servers|
| Reverse Proxy Server  |  A **reverse proxy server** is a type of proxy server that typically sits behind the firewall in a private network and directs client requests to the appropriate backend server    |
| Nginx |  **Nginx** is open source software for web serving |

---

## Documentation

Another element of this work is the creation of cowritten documentation, intended to allow others recreate and remix this work. It is available at:
<br/>

https://git.systerserver.net/queer/networks

https://github.com/In-grid-collective/servPubWiki

Soon to be hosted on servpub.net

---

## Collective working

Part of our process was working collectively on the command line, using a software to enable this:


![[Pasted image 20231123170353.png]]

We also spent many hours on _**etherpads**_ jotting notes and chatting through ideas, meeting in person and online using platforms like _**jitsi**_.


---

## Free and Open Source Software (FOSS)

Some examples of software you can host on a server: 

- [wiki-to-print](https://cc.vvvvvvaria.org/wiki/Wiki-to-print) by Creative Crowds usese [Mediawiki](https://www.mediawiki.org/wiki/MediaWiki) among other software
- [Gitea](https://about.gitea.com/) for hosting a lightweight Git platform
- [Etherpad](https://etherpad.org/) or [Cyrptpad](https://cryptpad.fr/) for collaborative workflows


---
## Hands on Activity 
Today we are going to be working on a Raspberry Pi locally. We are going to be using Node.js as for our servers.


---


## Accessing the Pi
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

__Directory__ : also known as a folder

Print working directory that you are currently in:

```shell
pwd
```


List current directory contents:

``` shell
ls
```

Print all invisible files:
``` shell
ls -a
```

--
### __Root directory__  
"Highest" directory in the hierarchy. You can also think of it as the start of a particular folder structure. Root directory of the user denoted by a tilde `~` on the command line. Change directory to root of the USER:

```shell
cd 
```

Then try `ls` to see what's there, next try this:

```shell
cd /
```

Then try `ls` again. In your finder window on mac you can do `cmd + shift + .` to see invisible files in the finder.

--

### __directory path__  
Route to a folder

Change directory using the "path" to that folder/directory. If the path starts with `/`  it will start at the root directory:

```shell
cd /directoryname/anotherdirectory
```

Change directory to the directory above the one you are currently in:

```shell
cd ..
```


---

## SSH 

We have enabled SSH on the (Pi) and you will need an ssh client package (on your laptop). If you haven't already checked if you have ssh, try to check the ssh version:

``` shell
ssh -v
```


--

## Connecting from a client

The first thing we need to do is find out the IP address or hostname of the device we want to access - the Pi. We have already done this, for those in the room we have added the Pi address for you on the Pad. But let's look at the commands.

We can find the hostname of the pi:

```shell
hostname 
```


--
## Finding an IP

On Linux/Mac we can do:

```shell
ifconfig
```

On windows it's `ipconfig`

But we just need the pi address, on Linux we have a nice shorthand:

```shell
hostname -I
```


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
### Let's make a TMUX session

To access a shared session, we all have to be logged in as the same user simultaneously. The tmux session will be registered under that user. To list all the tmux sessions running you can do:

```shell 
tmux ls
```

This is the command to create a new session called "mysession":

```shell 
tmux new -s mysession
```


[TMUX cheatsheet](https://tmuxcheatsheet.com/)

--

### Don't exit the session!

Careful about writing "exit" now! You will lose the tmux session:

```shell 
exit
```

--


We have started a tmux session, and you can join with the following command:

```shell 
tmux attach -t mysession
```



> [!Note]
> Everyone in a Tmux session is acting as the same user. However we could create split screens and multiple panes within Tmux so different people can work on different things. 

--

Let's try these again. Don't forget about using `ls` to see what's in the folder:

```shell
pwd
```

 Change directory to root of the USER:

```shell
cd 
```

Then try `ls` to see what's there, next try this:

```shell
cd /
```

--
### Quick Note on SUDO

`sudo` stands for [Super User DO](https://www.geeksforgeeks.org/sudo-command-in-linux-with-examples/). This command is used to allow you to run commands that only super users are allowed to run, and is often followed by another command you want to run:

```shell
 sudo <command>
```


--

### With great power comes responsibility!

--

We will enter into sudo mode to run all commands as a super user run:

```shell
 sudo su
```

When you use `sudo` you will be prompted to enter your super user password to execute the command if you are logged in as a user that doesn't have sudo privileges. 

Type this to exit sudo and enter back into the main user:
```shell
 exit
```


--

### Raspberry Pi configuration

```shell
sudo raspi-config
```

Hostname is a nice quick thing to look at:

```shell
cat /etc/hosts
```

`cat` prints out the contents of the hosts file to the terminal.

---
## Server Software 


For the Servpub project we use [NginX](https://www.nginx.com/)  which is a web serving software. 

For today as many of you in this room are familiar with Node.js and p5.js we are going to use Node.js to serve up sites:

- We have installed Node.js and NPM as well as nvm (Node version manager)
- On the pis we have put a simple web-socket example, a static server serving up a p5 sketch, and one serving up just html.

Let's take a look at that.

--

### Node.js Reminder

Navigate to the folder you see with the package.json:

```shell
npm install
```

```shell
node app.js
```

Next in your browser go to IP or hostname + the port number

e.g.
raspberrypi.local:3000

run `ctrl+c` to stop the server

--
### Collaborative Work on the Command Line

Normally you'd spend your time on the command line / in TMUX configuring server environments. 

For today's session we are just going to do a little bit of editing of text files as we are offline & don't have time to do a step-by-step configuration activity. 

--

### Editing Text Files on the command line

Let's open it up a file in the Nano command line text editor to make some changes.

Change the port number of a node server:
```shell
nano app.js
```

Or add some text:
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

### !  be careful   !
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


--

###  Systemctl 

We can use [systemd](https://en.wikipedia.org/wiki/Systemd) the Linux system and service manager to automate things. We can use systemctl to manage systemd services. 

Let's look at a systemd service for keeping one of our node.js servers up and running.