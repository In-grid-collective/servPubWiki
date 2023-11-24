---
theme: moon
---
<header>
<link rel="stylesheet" media="screen" href="https://fontlibrary.org//face/generale-station" type="text/css"/>
</header>

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
	h1,h2,h3{
        font-family : "GeneraleStationRegular"!important;
        font-size: 1.5em!important;
}
	code{
	     padding: 0.5em 1em!important;
	}

.slide-background{
background: rgb(79,9,121); background: linear-gradient(180deg, rgba(79,9,121,1) 4%, rgba(52,1,45,1) 100%);

}

</style>


<!-- .slide: class="bg" -->
# VPN and Reverse Proxy Server workshop

---


# Intro




This is one of the last steps for setting up our server configuration, where we are linking our server on a pi to the VPN network hosted by systerserver's server: Jean. 

This will then mean that this pi can be used as a mobile server, running online but not from a set IP address/location. 



--

## Collaborations!

Todays workshop will follow along the work of the [Rosa zine](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf) made by our collaborators Systerserver & Varia. 

As we have been following these steps we have been adding our own approaches troubleshooting around accessibility, transformability, alternative imaginaries of CI

---
## So far

We have basically got a pi setup with:
- Armbian OS
- SSH access
- Several users setup with sudo rights
- NginX installed & pointing to a static site
--
## In this session
![[slideAsset 7.png]]
We will be setting up:
- The VPN  using  [Tinc](https://www.cyberciti.biz/faq/ubuntu-install-tinc-and-set-up-a-basic-vpn/)
-  Reverse Proxy Server using [NginX](https://www.nginx.com/resources/glossary/reverse-proxy-server/)

---

# Installing Tinc


Tinc is a Virtual Private Network (VPN) daemon that uses tunnelling and encryption to create a secure private network between nodes on the Internet

You need to install tinc for all nodes of the subnetwork
- personal machines 
- servers

--
## OS

Today we will be installing Tinc on Armbian (Linux).

They are a litle bit more complex but we have found methods for installing on other OS:
- Mac - our manual in the wiki. 
- Windows - documentation coming, but using ubuntu console [like this](https://www.microsoft.com/store/productId/9PDXGNCFSCZV?ocid=pdpshare)

--
## Install instructions for Linux

Lets jump onto the Pi!

--


### Install dependencies

We will be using apt to install dependencies before we download and install tinc. 

```shell 
sudo apt install build-essential automake libssl-dev liblzo2-dev libbz2-dev zlib1g-dev libncurses5-dev libreadline-dev 
```

>	We don't need to know too much what they do to be honest, just that they are needed to run Tinc

--
### Download and decompress Tinc installer

Navigate into a tmp folder. This is so our install packages are automatically deleted when we reboot. 

```shell
cd /tmp
```

Download tinc 1.1pre17 (every node needs the same version of tinc) with wget and uncompress the downloaded item to a folder with tar.

``` shell
wget https://www.tinc-vpn.org/packages/tinc-1.1pre17.tar.gz 
tar xvf tinc-1.1pre17.tar.gz
```

--

### See what we have got!

Let's check the new folder that is there. We're still inside /tmp. 
``` shell
ls
```

>You should see these files:
> - `tinc-1.1pre17`
> - `tinc-1.1pre17.tar.gz`

You can check what is in the new folder with

``` shell
ls tinc-1.1pre17
```

--

### Install Tinc

Navigate into the folder and run  the configure file to set tinc up.

``` shell
cd tinc-1.1pre17 
./configure 
```

Install tinc with make install command

```shell
make 
sudo make install
```

--
Now we have installed tinc, we can setup our configuration.

--
### Make configuration folder

Once installed, create a configuration directory. All configurations of tinc will happen in this folder. Using tinc subcommands (like invite / join) will result in changes to the files in this folder.

```shell
sudo mkdir -p /usr/local/etc/tinc/
```

--

#### Checkout executable

We can also see the tinc executable is installed in sbin where it should be.

``` shell
ls /usr/local/sbin/tinc 
```


> This means that you can only run tinc as sudo, since sbin directory can be ran only with sudo rights.

---
# Use UFW to open up the necessary firewall

In this step, you will set up the default firewall ufw on all your server. You'll add OpenSSH service, add tinc VPN port, then start and enable ufw firewall.

--
## Open the ports

First, add the OpenSSH service using the ufw command below. An output '**Rules updated**' confirms that the new rule was added to ufw.

``` shell
sudo ufw allow OpenSSH
```

Add the port 655 that will be used by tinc VPN by entering the following command.

``` shell
sudo ufw allow 655
```
--
## Enable UFW and check it

Now run the following ufw command to start and enable the ufw firewall. When prompted, input y to confirm and press ENTER to proceed.

``` shell
sudo ufw enable
```

Check its status with

``` shell
sudo ufw status
```

---
# creating the initial network on tinc and inviting nodes


This step only has to happen once on the server hosting the vpn. In our case, it was performed on Jean. 

We created a virtual network named systerserver on the public node. Once this network is created, we use the public node to "invite" other nodes, for instance servpub or wiki2print. Invited nodes can then use tinc's *join* command to use the invite link.  

--
# Initialising the VPN and node

This command adds nodes to the network, but on the first call initiates the network (our network has already been initiated but are now adding the wiki2print pi to it)

Simply write:

```shell
sudo tinc -n <NETNAME> init <NODENAME>
```

so we did 

``` shell
sudo tinc -n systerserver init wiki2print
``` 

--

Tinc stores all configuration in:
`/usr/local/etc/tinc/` 

This allows multiple private networks, each in a folder with the name of the network. e.g.:
`/usr/local/etc/tinc/systerserver` 

>If you mess something up, you can restart by deleting the `<NETNAME>` folder and initialising it again. 


--
### Tracking IP's

To add a new client to the network you need to assign them a subnet of the VPN. For instance is the VPN IP is 10.10.12.x, we can give the new client a subnet IP i.e. 10.10.12.1.

Create a text file to keep your list of private addresses. We create a file called `vpn-records` in `/usr/local/etc/tinc/<NETNAME>/`

--

#### Add our new IP record

Create/open the file with:

``` shell
sudo nano /usr/local/etc/tinc/<NETNAME>/vpn-records
```

We did:

``` shell
sudo nano /usr/local/etc/tinc/systerserver/vpn-records
```

Then choose and write in the IP for the node as below:
``` vpn-records
<subnet IP> - <NodeName>

e.g.
10.10.12.1 - servepub

```
--


#### With multiple subnets your file might look something like: 

```
#servers
10.10.12.1    -  servpub 
10.10.12.2    -  wiki2print

#users
10.10.12.20   -  User1 
10.10.12.21   -  User2
```


--

## Now to invite the new node!


On the first invite of a node it will initialise the network.

``` shell
sudo tinc -n <NETNAME> invite <NODENAME>
```

for instance, in our case if we want to invite a new client called 'servepub':

```shell 
sudo tinc -n systerserver invite wiki2print
```


the invite generated will be a long string of letters and numbers. It can then be passed over to servepub to run.

--

Example of given invite code: 
```
79.91.202.97/SVublahX7LapWXJdBzd03jNn48bxuN83jVE_23VnL
```

---

# Joining the VPN network from Pi

Now we will jump to our Pi (wiki2print) to join the VPN network.

To  join the network from the new client  use:

``` shell
sudo tinc -n <NETNAME> join <invite code>
```

so we did:

``` shell
sudo tinc -n systerserver join 79.91.202.97/SVublahX7LapWXJdBzd03jNn48bxuN83jVE_23VnL
```

--

## Set subnet IP

Then we will set the node VPN subnet ip address in the 10.10.12.x subnet, using the command below with the IP we assigned to them earlier in the tracking IP step. 

```shell
sudo tinc -n <NETWORK NAME> add subnet <IP>
```

For servepub it would look like:

```shell
sudo tinc -n systerserver add subnet 10.10.12.1
```

> If you did not create the invite, the sysadmin who did should have assign you an IP, if not maybe ask them

--
### Edit tinc-up file:

On your machine, go to the configuration folder if you're not already there:
``` shell
cd /usr/local/etc/tinc/<NETNAME>
```

you can use `ls -a` to see all files in there, then edit the file called tinc-up. We are using `nano` to do our text editing.

``` shell
sudo nano tinc-up
```

--

#### ad in the network details

Once the file is open, edit it so it looks like this, filling in the correct details:

``` tinc-up
#!/bin/sh
#echo 'Unconfigured tinc-up script, please edit '$0'!'
#ifconfig $INTERFACE <your vpn IP> netmask <netmask of whole VPN>
```

For servepub it would look like:
```
#!/bin/sh 
#echo 'Unconfigured tinc-up script, please edit '$0'!' 
ifconfig $INTERFACE 10.10.12.1 netmask 255.255.255.0
```

--

#### Test network

To test and run the service with debug:
``` shell
sudo tincd -n systerserver -D
```

> It should return no errors

--
# Start tincd

Tincd is the daemon that runs our Tinc VPN automatically in the background.

This can be done on all systems in the network but today we are doing it on the Pi.

--

## Create a systemd service file

Systemd is a way to manage (start/stop) services like servers. Tinc is such a service. You can create new service files in the folder
`/etc/systemd/system`

In this case we create a special kind of service file (that has an @ in the name) that allows it to work for multiple network names.

```shell 
sudo nano /etc/systemd/system/tinc@.service
```

--
### write systemd service file

Inside that file, paste the following:

``` service
[Unit] 
Description=Tinc (%i) 
After=network.target 

[Service] 
Type=simple

# WorkingDirectory pointing to the configs at /usr/local/etc/tinc
WorkingDirectory=/usr/local/etc/tinc

# pointing to the tincd executable at /usr/local/sbin/tincd
ExecStart=/usr/local/sbin/tincd -D -n %i
ExecReload=/usr/local/sbin/tincd -D -n %i -kHUP 
TimeoutStopSec=5 
Restart=always 
RestartSec=60 

[Install] 
WantedBy=multi-user.target
```

--

## Start tinc@.service

Now we can start it with the command below @ our tinc NETNAME:

``` shell
sudo systemctl start tinc@<NETNAME>
```

so for us it would be:

``` shell
sudo systemctl start tinc@systerserver
```

To make the VPN start automatically on boot:
```shell 
sudo systemctl enable tinc@<NETNAME>
```

e.g.:

```shell 
sudo systemctl enable tinc@systerserver
```

---
# NginX Reverse Proxy

We are now going to run a reverse proxy server with NginX that will enamble us to forward our webtrafic from their public IP that the DNS server point to, to our internal vpn subnet IP.

#diagram

if you haven't already install Nginx, like we did for the pi [[02-Web Server Setup on Pi#NginX]]

--
## Reverse Proxy Configuration

On the server hosting the tinc vpn (in our case Jean) add an nginx config file at ``/etc/nginx/sites-available/<SERVERNAME>.conf``

To do this use the command

```shell
cd /ect/nginx/
sudo nano sites-available/<NETNAME>.conf
```

Ours looks like:

```shell
cd /ect/nginx/
sudo nano sites-available/systerserver.conf
```

--

### Decide on proxy configuration.

Choosing your NGINX reverse proxy setup is very much up to you but we will show two setups:
- A simple one with just http.
- A more secure and standard one with https redirect and certificate. 

--

#### simple http conf:

``` nginx
server {
	# listen to http on port 80
	listen 80;
	listen [::]:80;
	# listen to url
	server_name servpub.net;

	# linking the client to the vpn subnet ip address of the pi
	location / {
		proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;
		proxy_set_header X-Forwarded-For
		$proxy_add_x_forwarded_for;
		proxy_set_header X-Forwarded-Proto $scheme;

		# replace this with the user vpn subnet ip adress you set earlier 
		proxy_pass http://10.10.12.51;
		proxy_read_timeout 90;
	}
	
	location / {
		rewrite ^ https://$host$request_uri? permanent;
	}
}
```

--

#### More complex https, with http redirect, and certificate:

``` nginx

# A server to derirect http traffic to https
server {
		# listen on port 80 (http) of servpub.net
        listen 80;
        listen [::]:80;
        
        # listen to servpub, change to your url
        server_name servpub.net;

		# send trafic to https of the url
        return 301 https://servpub.net$request_uri;

}

server{
        # SSL configuration, listen to https of port 443
         listen 443 ssl default_server;
         listen [::]:443 ssl default_server;

		# link to ssl certificate on vpn server
         ssl_certificate /etc/letsencrypt/live/servpub.net/fullchain.pem;
         ssl_certificate_key /etc/letsencrypt/live/servpub.net/privkey.pem;

		# link to root folder
        root /var/www/html;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

		# listen to servpub, change to your url. 
        server_name servpub.net;

		# linking the client to the vpn subnet ip address of the pi
        location / {
                proxy_set_header        Host $host;
                proxy_set_header        X-Real-IP $remote_addr;
                proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header        X-Forwarded-Proto $scheme;

				# replace this with the user vpn subnet ip adress you set earlier 
                proxy_pass              http://10.10.12.53; 
                
                proxy_read_timeout      90;

                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;


        }
        
}


```

--

### Debuging NginX

You should be able to find information about errors and traffic by using nano to look here:

`/var/log/nginx/access.log`

`/var/log/nginx/error.log`

--

### Enable the NginX proxy

We do this by linking our nginx config file at:

`/etc/nginx/sites-available/<SERVERNAME>.conf` 

to its neighbour folder at: 

`/etc/nginx/sites-enabled/<SERVERNAME>.conf` 

--

#### linking the nginx .conf

To do this use the ln command:

```shell
sudo ln sites-available/<NETNAME>.conf sites-enabled/<NETNAME>.conf
```

Ours looks like:

```shell
sudo ln sites-available/systerserver.conf sites-enabled/systerserver.conf
```

---

# It should be setup!

Go checkout [wiki2print.servepub.net](wiki2print.servepub.net)
