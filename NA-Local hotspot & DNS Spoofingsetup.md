
edited version from [HERE](https://readwrite.com/tutorial-setup-raspberry-pi-wifi-hotspot-access-point/)

also with input form[HERE](https://semfionetworks.com/blog/wlan-pi-setup-a-wi-fi-hotspot/) and  [HERE](https://www.instructables.com/Raspberry-Pi-Web-Server-Wireless-Access-Point-WAP/)

This tutorial expects that you have a setup a pi with:
- An OS installed
- Nginx setup
- A software to connect to
- direct access to

In this tutorial we are going to:
- Turn the pi's WIFI into a hotspot.
- Use Dnsmasq and Nginx port forwarding to spoof domains on our local network.

## Prerequisites
### Update

upgrade your software

``` shell
sudo apt-get update
sudo apt-get upgrade
```
### Install Software

Install:
- [Hostapd](https://wiki.gentoo.org/wiki/Hostapd) - Enables us to turn WIFI receivers into WIFI router/transmitters
- [dnsmasq](https://dnsmasq.org/) - Enables us to create a local DNS server to reroute 

```shell
sudo apt-get install hostapd && sudo apt-get install dnsmasq && sudo apt install dnsutils
```

If you want to check they have installed use the command below!

```shell
dpkg –-list | grep hostapd
dpkg –-list | grep dnsmasq
dpkg –-list | grep dnsutils
```

Stop them whilst we edit their configurations.

```shell
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq
sudo systemctl stop dnsutils
```

## Turn Pi Into a WIFI hotspot
### Adding static IP

We are going to edit our dhcpcd .conf to create a static IP to configure the network to. Assuming that the standard `192.168.###.###` IP is being used for networking we are going to set up the WiFi IP to `192.168.0.1`

``` shell
sudo nano /etc/dhcpcd.conf
```

add these lines at the end of the file to set the ip to `192.168.0.1`.

``` conf
interface wlan0
    static ip_address=192.168.0.1/24
```

Restart dhcp

``` shell
sudo service dhcpcd restart
```

Check dhcpcd's status, it should say `active (runnning)` in green.

``` shell
sudo service dhcpcd status
```

### Config dnsmasq

The default Dnsmasq config has too many settings so we are going to make a copy of the original config encase, then edit a new fresh empty one with nano.
``` shell 
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
sudo nano /etc/dnsmasq.conf
```

Give the IP addresses range for the wlan0 interface in the renamed file:
``` conf
interface=wlan0
 	dhcp-range=192.168.0.2,192.168.0.20,255.255.255.0,24h
```

### Config Hostapd

Now we will setup the WIFI hotspot settings by editing the Hostapd .conf file with nano. 
```shell
sudo nano /etc/hostapd/hostapd.conf
```

Paste in these network settings.

``` conf
## WLAN SSID
ssid= **Network Name**
 
## WPA-PSK
wpa_passphrase= **Password**
 
## Mode options: a=5GHz / g=2.4GHz
hw_mode=a
 
## Set 2.4GHz Channel - 1,6,11
## Set 5GHz Channel - 36,40,44,48,149,153,157,161,165
channel=36
 
## Set Interface and Driver to user
interface=wlan0
driver=nl80211 # you may need a different driver but this works with built in
 
## Set Country Code (Use your own country code here)
country_code=GB
 
## IEEE 802.11n SETTINGS
ieee80211n=1
#ht_capab=[HT40+][SHORT-GI-20][SHORT-GI-40][DSSS_CCK-40]
 
## IEEE 802.11ac SETTINGS
ieee80211ac=1
#vht_oper_chwidth=1
vht_capab=[SU-BEAMFORMEE-1][HTC-VHT-1][VHT-LINK-ADAPT2][MAX-AMSDU-7935][GF]
#vht_oper_centr_freq_seg0_idx=42
 
## Set Security Parameters (WPA2-Personal here)
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
 
## Enable WMM :P
wmm_enabled=1
uapsd_advertisement_enabled=1
```

Edit the:
- SSID - the host name which will broadcast to the other devices
- _wpa_passphrase_ - the password

More docs settings [HERE](https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf)

Now we need to link this hostapd.conf as our+ default:

```shell
sudo nano /etc/default/hostapd
```

Uncomment the `DAEMON_CONF= ” ”` and in the " " add the path to the configuration we just wrote so it looks like below.

``` hostapd
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

### Enable Ip Forwarding

This enables our WIFI hotspot to connect through to our internet source.

To do this just simply edit Systctk's .conf
``` shell
sudo nano /etc/sysctl.conf
```

and uncommenting ip_forwarding like bellow.
``` conf
net.ipv4.ip_forward=1
```

### Start Services

Now we are done editing start the hostapd and dnsmasq services by:
``` shell
sudo service hostapd start
sudo service dnsmasq start
```

### Configuring NAT(Network Address Translation)
NAT(Network Address Translation) will forward all the traffic from your access point over to your ethernet connection. This can be done by:
``` shell
sudo iptables -t nat -A  POSTROUTING -o eth0 -j MASQUERADE
```

Save it:
``` shell
sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
```

To make your Pi run as an access point by default after every restart, we need to edit the rc.local file:
``` shell
sudo nano /etc/rc.local
```

Add these commands just above “exit 0”:
``` local
iptables-restore < /etc/iptables.ipv4.nat
```
### Bridge Traffic from AP to Ethernet

The Pi should now work as a WiFi access point and you can reboot the Pi and connect to its WiFi from another device. This can work great as a local network but you still won’t be able to connect to the internet as we haven’t yet bridged the incoming traffic from the WiFi AP to the Ethernet’s internet connection. 

To do that:

``` shell
sudo apt-get install hostapd bridge-utils
```


Now, we add a new bridge:
``` shell
sudo brctl addbr br0
```

Connect Ethernet to the bridge by:
``` shell
sudo brctl addif br0 eth0
```

### Add the new bridge interface 
The interface needs to be edited to make the connection between the WiFi access point and ethernet work. This is done by:
``` shell
sudo nano /etc/network/interfaces
```


Add the bridging rule:
``` interfaces
# Bridge setup
auto br0
iface br0 inet manual
bridge_ports eth0 wlan0
```

### Test it
Reboot and see if you can connect to the WIFI + internet!
```shell
sudo reboot
```

## DNS Spoofing + NGINX Port Forwarding  To Redirect Traffic From URLs To Ports.

modified from [HERE](https://dev.to/ivishalgandhi/local-home-lab-dns-setup-with-dnsmasq-and-nginx-8b5)

The next step is to enable our local WIFI network to reroute URLs and traffic from domain names towards specific programs running on local ports. If your program on this server is already hosted online their may be no point in this but if it is a local setup it can make things more accessible. You might want to do this with a program like the one installed with [[NA-Suggested tools to host]], but it can be done to any other program running through a local port. 

The options to access container applications before implementing DNSMasq and NGINX

`writefreely` - [http://localhost:8080](http://localhost:8080)

after implementation of DNSMasq and NGINX

`writefreely` - http://write.example.net

### Create a DNSMasq configuration 

Create a new configuration file with DNSMasq for it to reroute traffic with. Replace  `example.net` domain name with the one you desire.

``` shell
sudo nano /etc/dnsmasq.d/<example.net>
```

Paste and edit the config below:

``` conf
no-dhcp-interface=enp2s0f0
bogus-priv
domain=<example.net>
expand-hosts
local=/<example.net>/
domain-needed
no-resolv
no-poll
server=8.8.8.8
server=8.8.4.4
```

Restart DNSMasq:

``` shell
   sudo systemctl restart dnsmasq
```


### Add DNS Container Records

This is to create a record of the subdomain that  we want it to be hosted at (e.g. "write" from http://write.example.net). We do this by editing our hosts configuration file. We earlier set our static IP, but you may want to check it again quickly. 

``` shell
hostname -I
```

If you have been following from the start of this page it should show our IP as `192.168.0.1` but if it is different stick with your one. 

``` shell
  sudo nano /etc/hosts  
```

Then below all the existing settings, we add the IP of our machine assigned to the subdomain. 

``` hosts
# add to below the other settings

192.168.0.1   write
```

Restart DNSMasq

``` shell
sudo systemctl restart dnsmasq.service
```

### NGINX Reverse Proxy

This step expects that you have a program installed on your server and it is running on a port. We are running Writefreely installed from the tutorial [[NA-Suggested tools to host]] and it runs on port `8080`. 

Firstly we will create a new sites available conf for NGINX, replacing `example` with the name of the site:

``` shell
  sudo nano /etc/nginx/sites-enabled/example.conf
```

This is a very basic example that will forward the ports, and not much else. The key extra here is the `proxy_bind` attribute as this helps with the redirection. 

Make sure to set:
-  `server_name` - to the url you chose
- `proxy_bind` - to the IP of your machine
- `proxy_pass` - to the right port for the software you are using. 

``` conf
server {
	listen 80;
	listen [::]:80;
	server_name write.example.net;
	location / {
		proxy_bind 192.168.0.1;
		proxy_pass http://localhost:8080;
	}
}
```

Most software will provide you with their own NGINX .conf file and you should be able to edit is by adding `proxy_bind 192.168.0.1;` to the top of the location sections. Below is an edited version on the writefreely NGINX .conf that works with the DNSMasq setup. Don't Forget to change the other parameters on listed on the example above!

``` conf

server {
    listen 80;
    listen [::]:80;

    server_name example.net;

    gzip on;
    gzip_types
      application/javascript
      application/x-javascript
      application/json
      application/rss+xml
      application/xml
      image/svg+xml
      image/x-icon
      application/vnd.ms-fontobject
      application/font-sfnt
      text/css
      text/plain;
    gzip_min_length 256;
    gzip_comp_level 5;
    gzip_http_version 1.1;
    gzip_vary on;

    location ~ ^/.well-known/(webfinger|nodeinfo|host-meta) {
        proxy_bind 192.168.0.1; # <<<<<<<<<<<<<<< Added Here
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
    }

    location ~ ^/(css|img|js|fonts)/ {
        root /var/www/cozy.cloud.net/static;
        # Optionally cache these files in the browser:
        # expires 12M;
    }

    location / {
        proxy_bind 192.168.0.1; # <<<<<<<<<<<<<<< Added Here
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://127.0.0.1:8080;
        proxy_redirect off;
    }
}

```

