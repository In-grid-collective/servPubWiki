---
type: Documentation
created: Tuesday 6th June 2023 21:13
project: ServPub
---

The next step is to setup the software needed to make this local server into a web server. 
# "Stack"

It seems like we'll need the following:
- Webserver (Apache or NginX)
- VPN ([tinc](https://www.cyberciti.biz/faq/ubuntu-install-tinc-and-set-up-a-basic-vpn/))
- [Tmux](https://tmuxcheatsheet.com/quick-start/)
- Ufw (Uncomplicated firewall)
# Webserver Software

Apache and NginX seem to be the two main competitors for web server software. There main pros and cons are outlined in [this article](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The%20main%20difference%20between%20NGINX,to%20have%20generally%20better%20performance) and [this article](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The%20main%20difference%20between%20NGINX,to%20have%20generally%20better%20performance.).

The gist is that NginX seems to be "better" in terms of performance and speed, but it doesn't seem to have full support on windows computers and needs an external processor for dynamic websites (not sure if we need that though). Basically NginX is faster because it does less out of the box.

# NginX

We will put NginX on our server! though it seems that we may need Apache too, later.

``` shell
apt install nginx
```

Note: it is saved by default in the /home directory. To find the HTML index page go to: /home/var/www/html. 

The document will be named by default as "index.nginx-debian.html"

Before doing anything else, consider whether to relocate nginx and how to name the documents. 

_currently, the file still needs editing and renaming_

(more soon)

# Tmux installation

Tmux is a terminal multiplexer, for collective editing in a terminal. Install this on the server machine. To use it you must be SSHed into the server.

So, assuming you are not SSHed into your server, follow these steps.

To install:

``` shell
apt-get install tmux
```

To create a new session:

``` shell
tmux new -s [name]  
```

example: tmux new -s pretest

To connect (or reconnect) to a session by anyone who is sshed into the server:

``` shell
tmux a -t [name] 
```

example: tmux a -t pretest

Note: Everyone in a Tmux session is acting as the same user, but we can create split screens and multiple panes within Tmux so potentially different people can work on different things. 

Reference and troubleshooting: https://www.howtogeek.com/devops/how-to-get-started-and-use-tmux/

# Tinc

## on Linux:
The tinc instructions in this guide zine work well [https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf) 

## on Mac:
We had to combine several instructions guides with the Rosa zine guide (Above). The reason it is a bit tricky on Mac is that we need ***tinc version 1.1*** and macs default installers (like homebrew) will always download the next latest version instead. 

### Prerequisites: 
- [Xcode](https://developer.apple.com/xcode/) (requires free online ADC Membership it can also be obtained from original OSX installation DVD. 
- [Macports](http://www.macports.org/install.php)

After Macports is installed, close and reopen your terminal. Update the ports system and ports list:

``` shell
sudo port selfupdate
sudo port sync
```

To install tinc-devel using macports run this command found at the link below:

``` shell
sudo port install tinc-devel
```

Configuration files will be located in /opt/local/etc/tinc

Tinc can now be configured and executed.
### Configuration:
To create a configuration file, you must join the tinc service through an invite link generated on the server machine running the service, in our case, Jean. Once you have generated your invite code, run:
``` shell
sudo tinc -n systerserver join <invite code>
```

This should then generate your folder "systerserver" with the configured files for our service: **/opt/local/etc/tinc/systerserver**

After the invite has been accepted you need to add your subnet / IP to systerserver service, i.e. _use the vpn IP address we set up on Jean for you (ask someone if you don't know what it is or find it on jean here: **usr/local/etc/tinc/systerserver/vpn-records**)_:
``` shell
sudo tinc -n systerserver add subnet <your assigned vpn IP>
```

### Edit tinc-up:
on your machine, go to the configuration folder if you're not already there:
``` shell
cd /opt/local/etc/tinc/systerserver
```

you can use ls -a to see all files in there, then edit the file called tinc-up. You can do this using vim or nano (both work on mac & are just different ways to edit a file in the terminal)

via [[vim]]: #maybe_remove overcomplicates + we are using nano in pages before. if remove don't forget the bit below.

``` shell
sudo vim tinc-up
```

via nano:
``` shell
sudo nano tinc-up
```

Your file should then look like the text below, in the code block, but swap out whats between the < and > with the vpn IP address we set up on Jean for you (ask someone if you don't know what it is)

``` tinc-up
#!/bin/sh
#echo 'Unconfigured tinc-up script, please edit '$0'!'
#ifconfig $INTERFACE <your vpn IP address> netmask <netmask of whole VPN>
ifconfig $INTERFACE <your vpn IP address> 10.10.12.2 up netmask 255.255.255.0
```

Exit from vim:
- To save, first press **esc (escape**)
- Enter **:wq**  (which stand for write and quite)
- Press enter

Exit from nano:
- Ctr+X
- type "y" to save changes
- press enter

To test and run the service with debug:
``` shell
sudo tincd -n systerserver -D
```

# Troubleshooting 
