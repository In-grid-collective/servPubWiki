
This is a very easy way of setting up a local wifi hotspot from the pi!

To do this we will be running the `armbian-config` and running through some gui options (very easy).

## open the Armbian config

First of all open up `armbian-config` with:
```shell
sudo armbian-config
```

It will take a second to load but will pop up with a screen like this:
![[armbian-config screenshot.png]]

**Note:** If this fails to open you can try to install armbian-config on other Debian distributions from this [REPO](https://github.com/armbian/config). If you cannot get it to work on your system, then you won't be able to carry on.
## Setup hotspot

navigate to through the menu to `Netork > Hotspot` and follow the steps. Each step may take a second to actuate but the questions are as follows (and confusingly have the same titles):

**1. Select interface:**
This is which interface you want to use as a hotspot. I chose wlan0 as it is the inbuilt wifi.

**2. Select interface:**
This is which interface you want to port the internet from. I chose eth0 as this is the ethernet, but you could choose a dongle here or other input. 

You have a hotspot working!
### Setting network details

You should now be back in the config menu again, now select `hotspot` again, and this time it will give you the option to `stop` or `edit`. Select `edit`.

now select the input interface we chose before, in my case the eth0.

It will now ask if we want to do a `Basic` or `Advanced` edit, we just want to do a `basic` one.

Now input the `SSID` (name of the network), and the password. 

Your network is now setup! try connecting to it. 