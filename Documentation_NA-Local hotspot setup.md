
edited version from [HERE](https://readwrite.com/tutorial-setup-raspberry-pi-wifi-hotspot-access-point/)

also with input form[HERE](https://semfionetworks.com/blog/wlan-pi-setup-a-wi-fi-hotspot/) and  [HERE](https://www.instructables.com/Raspberry-Pi-Web-Server-Wireless-Access-Point-WAP/)

### Update

``` shell
sudo apt-get update
sudo apt-get upgrade
```
## Install Hostapd

This software enables you to setup local WIFI hotspots.

```shell
sudo apt-get install hostapd
sudo apt-get install dnsmasq
```

Check its installed
```shell
dpkg –list | grep hostapd
dpkg –list | grep dnsmasq
```

Stop the hostapd +dnsmasq whilst editing it
```shell
sudo systemctl stop hostapd
sudo systemctl stop dnsmasq
```

### Adding static IP

``` shell
sudo nano /etc/dhcpcd.conf
```

add these lines at the end of the file.
``` conf
interface wlan0
    static ip_address=192.168.0.1/24
```

Restart dhcp
``` shell
sudo service dhcpcd restart
```

### Config dnsmasq

make a copy of the original config encase, then edit a new one
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

open the conf file with nano. 
```shell
sudo nano /etc/hostapd/hostapd.conf
```

Setup network settings.

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
country_code=CA
 
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

More docs settings [HERE](https://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf)

set up the daemon to launch it as a default
```shell
sudo nano /etc/default/hostapd
```

In DAEMON_CONF=” ” add
``` hostapd
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

### Enable Ip Forwarding

To do this just simply edit systctk
``` shell
sudo nano /etc/sysctl.conf
```

and uncommenting ip_forwarding like bellow.
``` conf
net.ipv4.ip_forward=1
```

### Start Services

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
We are done setting the Pi as a WiFi access point and you can reboot the Pi and connect to its WiFi from another device. But you still won’t be able to connect to the internet as we haven’t yet bridged the incoming traffic from the WiFi AP to the Ethernet’s internet connection. To do that:
``` shell
sudo apt-get install hostapd bridge-utils
```

Again, stop hostapd for configuration by:
``` shell
sudo systemctl stop hostapd
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
Reboot and see if you can connect
```shell
sudo reboot
```


next bit is dns spoofing? or is it adding access to pages:
https://www.linux.com/topic/networking/dns-spoofing-dnsmasq/