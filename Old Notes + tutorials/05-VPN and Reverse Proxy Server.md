

# "Stack"

We will need the following:
- VPN ([tinc](https://www.cyberciti.biz/faq/ubuntu-install-tinc-and-set-up-a-basic-vpn/))
-  [Reverse Proxy Server](https://www.nginx.com/resources/glossary/reverse-proxy-server/) (NginX)


[Notes](https://pad.riseup.net/p/un-Named_Server_CCI-keep)

# Tinc

tinc is a Virtual Private Network (VPN) daemon that uses tunnelling and encryption to create a secure private network between hosts on the Internet. Find out [more](https://www.tinc-vpn.org/) 

You need to install tinc for all nodes, both server and client. 

If you would like to install on Mac, check out [[NA-Installing tinc on Mac.md.md|NA-Installing tinc on Mac]]. It's a little bit more complicated than Linux. If you want to install on Windows, it may be better to run tinc from a linux console [like this](https://www.microsoft.com/store/productId/9PDXGNCFSCZV?ocid=pdpshare)

## Install instructions for Linux:
The tinc install instructions in this guide zine work well. This is what we have used to install tinc on a pi. [https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf) 

We will be using apt to install development tools and dependencies before we download the source code of tinc. 

First we will download essential packages to  compile tinc as a sudo user.

```shell 
sudo apt install build-essential automake libssl-dev liblzo2-dev libbz2-dev zlib1g-dev libncurses5-dev libreadline-dev 
```

Navigate into a tmp folder.

```shell
cd /tmp
```

Download tinc 1.1 with wget and uncompres the downloaded item to a folder with tar.

``` shell
wget https://www.tinc-vpn.org/packages/tinc-1.1pre17.tar.gz 
tar xvf tinc-1.1pre17.tar.gz
```

Navigate into the folder and run  the configure file to set tinc up.

``` shell
cd tinc-1.1pre17 
./configure 
```

Install tinc!

```shell
make 
sudo make install
```

Once installed, create a configuration directory. All configurations of tinc will happen in this folder. Using tinc subcommands (like invite / join), result in changes to the files in this folder.

```shell
sudo mkdir -p /usr/local/etc/tinc/
```

The tinc executable is installed in 

`/usr/local/sbin/tinc `

This means that you can only run tinc as sudo, since sbin directory saves binary executables that can be ran only by sudo (s+bin)

### Use UFW to open up the necessary firewall

In this step, you will set up the default firewall ufw on all your server. You'll add OpenSSH service, add tinc VPN port, then start and enable ufw firewall.

First, add the OpenSSH service using the ufw command below. An output '**Rules updated**' confirms that the new rule was added to ufw.

``` shell
sudo ufw allow OpenSSH
```

Add the port 655 that will be used by tinc VPN by entering the following command.

``` shell
sudo ufw allow 655
```

Now run the following ufw command to start and enable the ufw firewall. When prompted, input y to confirm and press ENTER to proceed.

``` shell
sudo ufw enable
```

Check its status with

``` shell
sudo ufw status
```

Tinc stores all configuration in `/usr/local/etc/tinc` Tinc allows multiple private networks to be defined, each is a folder with the name of the network, e.g. `/usr/local/etc/tinc/jean` (If you mess something up, you can restart by deleting the files that are there).

## Tinc: creating the initial network and inviting nodes


This step only has to happen once on the server hosting the vpn. In our case, it was performed on Jean. 

We created a virtual network named "constant" on the public node. Once this network is created, we use the public node to "invite" other nodes, for instance your laptop into this network. Invited nodes can then use tinc's *join* command to use the invite link. The generic form for initializing a new network named NETNAME is 

```shell
sudo tinc -n <NETNAME> init <NODENAME>
```

so we did 

``` shell
sudo tinc -n systerserver init servpub
```

To add a new client to the network you need to assign them a subnet of the VPN. For instance is the VPN IP is 10.10.12.x, we can give the new client a subnet IP i.e. 10.10.12.1.

Create a file to keep your list of private addresses. We use a file called `vpn-records` in `/usr/local/etc/tinc/`

Your file might look something like: 
10.10.12.1      servpub 
10.10.12.52    NewUser1 
10.10.12.53    NewUser2

run

``` shell
sudo tinc -n <NETNAME> invite <CLIENTNAME>
```

for instance, in our case if we want to invite a new client called 'servepub':

```shell 
sudo tinc -n systerserver invite servepub
```


the invite generated will be a long string of letters and numbers. It can then be passed over to servepub to run.

Example of given code: 
`79.91.202.97/SVublahX7LapWXJdBzd03jNn48bxuN83jVE_23VnL`

To  join the network use:

``` shell
sudo tinc -n <NETNAME> join <invite code>
```

so we did:

``` shell
sudo tinc -n systerserver join 79.91.202.97/SVublahX7LapWXJdBzd03jNn48bxuN83jVE_23VnL
```

servepub will then be asked to give their sudo password.

Then servepub will set the VPN ip address in the 10.10.12.x subnet, using the command below and the IP assigned to them e.g.

```shell
sudo tinc -n <NETWORK NAME> add subnet <IP>
```

For servepub it would look like:

```shell
sudo tinc -n systerserver add subnet 10.10.12.1
```

### Edit tinc-up:
On your machine, go to the configuration folder if you're not already there:
``` shell
cd /usr/local/etc/tinc/<NETNAME>
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

For servepub it would look like:
```
#!/bin/sh 
#echo 'Unconfigured tinc-up script, please edit '$0'!' 
ifconfig $INTERFACE 10.10.12.1 netmask 255.255.255.0
```


To test and run the service with debug:
``` shell
sudo tincd -n systerserver -D
```


### Start tinc

After this, make sure that the server and the client are both running tinc. we are now going to get tinc to launch automatically with [systemd](https://en.wikipedia.org/wiki/Systemd) 

#### Create a systemd service file

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

Now we can start it with the command below:

``` shell
sudo systemctl start tinc@<NETNAME>
```

so for us it would be:

``` shell
sudo systemctl start tinc@systerserver
```

To make the VPN start automatically
```shell 
sudo systemctl enable tinc@<NETNAME>
```

e.g.:

```shell 
sudo systemctl enable tinc@systerserver
```

# NginX for Reverse Proxy

Apache and NginX seem to be the two main competitors for web server software. There main pros and cons are outlined in [[https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The main difference between NGINX,to have generally better performance]] and [[https://www.hostinger.co.uk/tutorials/nginx-vs-apache-what-to-use/#:~:text=The main difference between NGINX,to have generally better performance.]].

The gist is that NginX seems to be "better" in terms of performance and speed, but it doesn't seem to have full support on windows computers and needs an external processor for dynamic websites (not sure if we need that though). Basically NginX is faster because it does less out of the box, which is why we will be using it now.

if you havent already install Nginx, like we did for the pi [[02-Web Server Setup on Pi.md.md#NginX]]

## Reverse Proxy Configuration

Make sure you have installed Nginx. If you do not have it installed refer to [[02-Web Server Setup on Pi.md.md#NGINX]]

Add an nginx config file at ``/etc/nginx/sites-available/<SERVERNAME>.conf``

To do this use the command

```shell
cd /ect/nginx/
nano sites-available/<NETNAME>.conf
```

Ours looks like:

```shell
cd /ect/nginx/
nano sites-available/systerserver.conf
```

Choosing your NGINX reverse proxy setup is very much up to you but we will show to setups, a simple one with just http, and a more secure and standard one with https redirect and certificate. 

simple http conf:

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

More complex https, with http redirect, and certificate:

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
         listen 443 ssl;
         listen [::]:443 ssl;

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


You should be able to find information about errors and traffic here:
`/var/log/nginx/access.log`
`/var/log/nginx/error.log`

