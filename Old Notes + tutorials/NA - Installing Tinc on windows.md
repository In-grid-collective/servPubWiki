
The easiest version we found for for this (as they do have a windows version) is to actually install it via an ubuntu virtual shell that is available on the windows store. Then follow the usual Linux install!

I also cant get it to run automatically but there is a minor overview at [[NA - Installing Tinc on windows.md.md#Difference in Systemctl commands]]

## Install ubuntu VM shell

install the VM from the windows store [HERE](https://www.microsoft.com/store/productId/9PDXGNCFSCZV?ocid=pdpshare)and remember your password!

## Install net-tools

This is needed to run some ifconfig commands on tinc-up file. but can be installed any time before. 

``` shell
sudo apt update
sudo apt install net-tools
```

## Install tinc

Then follow the [[05-VPN and Reverse Proxy Server.md.md#Install instructions for Linux]], and come back here when you reach [[05-VPN and Reverse Proxy Server.md.md#Start tinc]], as you cannot automatically run tinc with systemd on the vm. Instead run the command below every time you want to start it.

## Run tinc

At the moment, I can't work out how to do sysvinit instead of systemd. There are some notes in difference in commands but I think the file needs rewriting in a different place as well. 

For the time being you can start the daemon manually with the command below:
``` shell
 tinc start -n systerserver
```
## Difference in automation commands

The main difference is that it uses Sysvinit instead of systemd. This is due to it using an older system for some reason.

Exchange the commands for the ones needed in the table bellow.

|Systemd command|Sysvinit command|
|---|---|
|systemctl start service_name|service service_name start|
|systemctl stop service_name|service service_name stop|
|systemctl restart service_name|service service_name restart|
|systemctl status service_name|service service_name status|
|systemctl enable service_name|chkconfig service_name on|
|systemctl disable service_name|chkconfig service_name off|

We may also need to rewrite or edit the conf file command to get this to work as well.