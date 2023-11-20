

# "Stack"

We will need the following:
- VPN ([tinc](https://www.cyberciti.biz/faq/ubuntu-install-tinc-and-set-up-a-basic-vpn/))
-  [Reverse Proxy Server](https://www.nginx.com/resources/glossary/reverse-proxy-server/) (NginX)


[Notes](https://pad.riseup.net/p/un-Named_Server_CCI-keep)

# Tinc

tinc is a Virtual Private Network (VPN) daemon that uses tunnelling and encryption to create a secure private network between hosts on the Internet. Find out [more](https://www.tinc-vpn.org/) 

You need to install tinc for all nodes, both server and client. 

If you would like to install on Mac, check out [[NA-Installing tinc on Mac]]. It's a little bit more complicated than Linux. If you want to install on Windows, it may be better to run tinc from a linux console [like this](https://www.microsoft.com/store/productId/9PDXGNCFSCZV?ocid=pdpshare)

## Install instructions for Linux:
The tinc install instructions in this guide zine work well. This is what we have used to install tinc on a pi. [https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf) 

We will be using apt to install development tools and dependencies before we download the source code of tinc. 

First we will download essential packages to  compile tinc as a sudo user.

```shell 
sudo su

apt install build-essential automake libssl-dev liblzo2-dev libbz2-dev zlib1g-dev libncurses5-dev libreadline-dev 
```

Navigate into a tmp folder.

```shell
cd ./tmp
```

Download tinc 1.1 and uncompress the folder

``` shell
wget https://www.tinc-vpn.org/packages/tinc-1.1pre17.tar.gz 

tar xvf tinc-1.1pre17.tar.gz
```

Navigate into the folder and install the packages

```shell
cd tinc-1.1pre17 
./configure 
make 
make install
```

Once installed, create a configuration directory. All configurations of tinc will happen in this folder. Using tinc subcommands (like invite / join), result in changes to the files in this folder.

```shell
mkdir -p /usr/local/etc/tinc/
```

The tinc executable is installed in 

`/usr/local/sbin/tinc `

This means that you can only run tinc as sudo, since sbin directory saves binary executables that can be ran only by sudo (s+bin)

### Create a systemd service file

Systemd is a way to manage (start/stop) services like servers. Tinc is such a service. You can create new service files in the folder
`/etc/systemd/system`

In this case we create a special kind of service file (that has an @ in the name) that allows it to work for multiple network names.

```shell 
sudo nano /etc/systemd/system/tinc@.service
```

Inside that file, paste the following:

``` service
[Unit] 
Description=Tinc (%i) 
After=network.target 
[Service] 
Type=simple
WorkingDirectory=/usr/local/etc/tinc
ExecStart=/usr/local/sbin/tincd -D -n %i
ExecReload=/usr/local/sbin/tincd -D -n %i -kHUP 
TimeoutStopSec=5 
Restart=always 
RestartSec=60 
[Install] 
WantedBy=multi-user.target
```

Tinc stores all configuration in `/usr/local/etc/tinc` Tinc allows multiple private networks to be defined, each is a folder with the name of the network in `/usr/local/etc/tinc` (If you mess something up, you can restart by deleting the files that are there).

## Tinc: creating the initial network and inviting nodes

This step only has to happen once on the server hosting the vpn. In our case, it was performed on Jean. 

We created a virtual network named "constant" on the public node. Once this network is created, we use the public node to "invite" other nodes, for instance your laptop into this network. Invited nodes can then use tinc's *join* command to use the invite link. The generic form for initializing a new network named NETNAME is 

```shell
sudo tinc -n NETNAME init NODENAME
```

so we did 

``` shell
sudo tinc -n systerserver init servpub
```

To add a new client to the network you need to
assign them a subnet of the VPN. For instance is the VPN IP is 10.10.12.x, we can give the new client a subnet IP i.e. 10.10.12.01.

Create a file to keep your list of private addresses. We use a file called `vpn-records` in `/usr/local/etc/tinc/`

Your file might look something like: 
10.10.12.1      servpub 
10.10.12.52    NewUser1 
10.10.12.53    NewUser2

run `sudo tinc -n VPNNAME invite CLIENTNAME`

for instance, in our case if we want to invite a new client called 'NewUser1':

```shell 
sudo tinc -n systerserver invite newuser1
```


the invite generated will be a long string of letters and numbers. It can then be passed over to NewUser1 to run

i.e. 

```
sudo tinc -n systerserver join 79.99.202.57/zVublahX7LaCWXJdBzd03jNn48bxuN83jVE_26VnL
```

NewUser1 will then be asked to give their sudo password.

Then NewUser1 will set the VPN ip address in the 10.10.12.x subnet, using the command below and the IP assigned to them e.g.

```shell
sudo tinc -n <NETWORK NAME> add subnet <IP>
```

For NewUser1 it would look like:

```shell
sudo tinc -n systerserver add subnet 10.10.12.52
```

## Configuration:
To create a configuration file, you must join the tinc service through an invite link generated on the server machine running the service, in our case, Jean. Details of how to generate an invite are above.

Once you have generated your invite code, run:
``` shell
sudo tinc -n systerserver join <invite code>
```

This should then generate your folder "systerserver" with the configured files for our service: **/opt/local/etc/tinc/systerserver**

After the invite has been accepted you need to add your subnet / IP to systerserver service, i.e. _use the vpn IP address we set up on Jean for you (ask someone if you don't know what it is or find it on jean here: **usr/local/etc/tinc/systerserver/vpn-records**)_:
``` shell
sudo tinc -n systerserver add subnet <your assigned vpn IP>
```

### Edit tinc-up:
On your machine, go to the configuration folder if you're not already there:
``` shell
cd /opt/local/etc/tinc/systerserver
```

you can use `ls -a` to see all files in there, then edit the file called tinc-up. We are using nano to do our text editing.

via nano:
``` shell
sudo nano tinc-up
```

Once the file is open, edit it so it looks like this, filling in the correct details:

``` tinc-up
#!/bin/sh
#echo 'Unconfigured tinc-up script, please edit '$0'!'
#ifconfig $INTERFACE <your vpn IP address> netmask <netmask of whole VPN>
```

For NewUser1 it would look like:
```
#!/bin/sh 
#echo 'Unconfigured tinc-up script, please edit '$0'!' 
ifconfig $INTERFACE 10.10.12.53 netmask 255.255.255.0
```


Exit from nano:
- Ctr+X
- type "y" to save changes
- press enter

To test and run the service with debug:
``` shell
sudo tincd -n systerserver -D
```


## Start tinc

After this, make sure that the server and the client are both running tinc. On linux, do this with the following command:

``` shell
sudo systemctl start tinc@.system
```
To make the VPN start automatically
```shell 
sudo systemctl enable tinc@.system
```


# NginX

Apache and NginX seem to be the two main competitors for web server software. There main pros and cons are outlined in [this article](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The%20main%20difference%20between%20NGINX,to%20have%20generally%20better%20performance) and [this article](https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The%20main%20difference%20between%20NGINX,to%20have%20generally%20better%20performance.).

The gist is that NginX seems to be "better" in terms of performance and speed, but it doesn't seem to have full support on windows computers and needs an external processor for dynamic websites (not sure if we need that though). Basically NginX is faster because it does less out of the box, which is why we will be using it now.

First install Nginx, like we did for the pi [[02-Web Server Setup on Pi#NginX]]




Reverse Proxy Configuration



You should be able to find information about errors and traffic here:
`/var/log/nginx/access.log`
`/var/log/nginx/error.log`

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