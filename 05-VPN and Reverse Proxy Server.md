

# "Stack"

We will need the following:
- VPN ([tinc](https://www.cyberciti.biz/faq/ubuntu-install-tinc-and-set-up-a-basic-vpn/))
-  [Reverse Proxy Server](https://www.nginx.com/resources/glossary/reverse-proxy-server/) (NginX)


[Notes](https://pad.riseup.net/p/un-Named_Server_CCI-keep)

# Tinc

tinc is a Virtual Private Network (VPN) daemon that uses tunnelling and encryption to create a secure private network between hosts on the Internet. Find out [more](https://www.tinc-vpn.org/) 
## Install instructions for Linux:
The tinc install instructions in this guide zine work well. This is what we have used to install tinc on a pi. [https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf) 

## Windows
We are currently unable to get a working version of tinc on windows, but we are working on it!

## Install instructions on Mac:
We combined several instructions guides with the Rosa zine guide (Above). The reason being is it is a bit tricky on Mac and we need ***tinc version 1.1*** and macs default installers (like homebrew) will always download the next latest version instead. 

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

Configuration files will be located in `/opt/local/etc/tinc`

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



# NginX

Apache and NginX seem to be the two main competitors for web server software. There main pros and cons are outlined in [this article](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The%20main%20difference%20between%20NGINX,to%20have%20generally%20better%20performance) and [this article](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The%20main%20difference%20between%20NGINX,to%20have%20generally%20better%20performance.).

The gist is that NginX seems to be "better" in terms of performance and speed, but it doesn't seem to have full support on windows computers and needs an external processor for dynamic websites (not sure if we need that though). Basically NginX is faster because it does less out of the box, which is why we will be using it now.

First install Nginx, like we did for the pi [[02-Web Server Setup on Pi#NginX]]

Reverse Proxy Configuration




http conf setup 

### http to https redirect

This is pretty simple just add a new server section to the top of the nginx .conf file

``` conf

server{
listen 80;
server_name <Your URL (e.g. serpub.net)>;
}

```

Then delete the last references to port 80 on the original  server