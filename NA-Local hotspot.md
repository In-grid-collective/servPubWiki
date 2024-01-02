
This is a very easy way of setting up a local wifi hotspot from the pi!

To do this we will be running the `armbian-config` and running through some gui options (very easy).

## open the armbian config

First of all open up `armbian-config` with:
```shell
sudo armbian-config
```

It will take a second to load but will pop up with a screen like this:
![[armbian-config screenshot.png]]

**Note:** If this fails to open you can try to install armbian-config on other Debian distributions with:
```shell
    sudo wget https://apt.armbian.com/armbian.key -O key
    sudo gpg --dearmor < key | sudo tee /usr/share/keyrings/armbian.gpg > /dev/null
    sudo chmod go+r /usr/share/keyrings/armbian.gpg
    sudo echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/armbian.gpg] http://apt.armbian.com $(lsb_release -cs) main  $(lsb_release -cs)-utils  $(lsb_release -cs)-desktop" | sudo tee /etc/apt/sources.list.d/armbian.list
    apt update
    apt install armbian-config
```


## Setup hotspot

navigate to through the menu to `Netork > Hotspot` and follow the steps. Each step may take a second to actuate but the questions are as follows (and confusingly have the same titles):

**1. Select interface:**
This is which interface you want to use as a hotspot. I chose wlan0 as it is the inbuilt wifi.

**2. Select interface:**
This is which interface you want to port the internet from. I chose eth0 as this is the ethernet, but you could choose a dongle here. 

You have a hotspot working!
### Setting network details

You should now be back in the config menu again, now select `hotspot` again, and this time it will give you the option to `stop` or `edit`. Select `edit`.

now select the input interface we chose before, in my case the eth0.

It will now as if we want to do a `Basic` or `Advanced` edit, we just want to do a `basic` one.

Now input the `SSID` (name of the network), and the password. 

Your network is now setup! try connecting to it. 