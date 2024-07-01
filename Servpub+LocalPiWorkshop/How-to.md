---
theme: moon
---
<header>
<link rel="stylesheet" media="screen" href="https://fontlibrary.org//face/generale-station" type="text/css"/>
</header>

<style>

	code.nginx{ 
		font-size: 0.4em!important; 
		line-height: 1.5em!important; 
	} 
	table{ 
		font-size: 0.5em!important; 
		margin: 40px 0!important;
		color:white!important;
	}

    
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
		color:white!important;
	}
	h1,h2,h3{
        font-family : "GeneraleStationRegular"!important;
        font-size: 1.5em!important;
}
	code{
	     padding: 0.5em 1em!important;
	}

.slide-background{
background: rgb(79,9,121); background: linear-gradient(180deg, rgba(79,9,121,1) 4%, rgba(52,1,45,1) 100%)!important;

}

</style>


## Getting Set Up


- Use the [Getting Started with Raspberry Pi](https://www.raspberrypi.com/documentation/computers/getting-started.html#getting-started-with-your-raspberry-pi) documentation
- Flash your Operating System(OS) to the SD card using [Raspberry Pi Imager](https://www.raspberrypi.com/documentation/computers/getting-started.html#install-using-imager). If you have enough space on your SD card, you can have the full Desktop Gui. Choose the recommended OS for your Pi version if you don't have strong preferences. 
- At the very least you will need a USB keyboard to get your pi on your wifi/local network before you can SSH into it. [More here](https://www.raspberrypi.com/documentation/computers/getting-started.html#keyboard)
- Once you SD card is ready with the OS, put the SD card back in your pi, plug in the pi to the power, and it will do it's "First Boot". The main things you need to do are [Set up the User](https://www.raspberrypi.com/documentation/computers/getting-started.html#user) and [Set up the Wifi](https://www.raspberrypi.com/documentation/computers/getting-started.html#wi-fi)

Note: If you like me don't have an external wired mouse, or only have one USB port for a keyboard (and no USB hub). You can do `ctrl + alt + t` to open up a terminal window and then do all the rest of the set up from the commandline.

The rest of the instructions will be on the commandline. 

NOTE: If you ever need to reboot the pi during this set up. You can do this lovely little command to safely restart your pi:

```
sudo reboot
```

--- 
## Enabling SSH on a pi
 

On the the pi, execute: 

```shell
sudo raspi-config
```

You navigate with the arrow buttons ← ↑  ↓  → and press ENTER to select an option. 

Look for _Interfacing Options_, find and select *SSH* and enable it then click OK to exit and finish.

Full documentation on remote access for pi [here](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) 

### Change the hostname of the pi

While you are in the raspi config. Set the hostname of the pi to something unique if you have multiple pis. You could leave it as the default.

Look for *System Options*, find and select *Hostname*. Type in your new hostname following the rules they provide. Then select *OK*.

You navigate with the arrow buttons ← ↑  ↓  → and press ENTER to select an option. 

Full documentation on System Options [here](https://www.raspberrypi.com/documentation/computers/configuration.html#system-options)

You can exit out of the config by selecting *Finish* and pressing ENTER.

Fun fact: You can confirm that your hostname has changed by printing out the contents of the hosts file to the terminal:

```shell
cat /etc/hosts
```

`cat` prints out the contents of the hosts file to the terminal.

## SSH requirements:

Being on the same network as the pi. Note: If you are moving around, get a dongle, tether form your phone data package or "share wifi" form your laptop.

Us the IP address or the hostname of the pi.

One way to find the IP is using this command on the command line on the pi:
```shell
hostname -I
```

To find the hostname:

```shell
hostname 
```


Full documentation on SSH with the pi [here](https://www.raspberrypi.com/documentation/computers/remote-access.html#ssh)

## Connecting from a client:

Now that we have the IP address, any computer can request to SSH into the Pi through these commands:
``` shell
ssh pi@<pi ip>
```
eg.
``` shell
ssh pi@100.10.1.90
```

OR using the hostname

``` shell
ssh <user>@<hostname>.local
```
eg.
``` shell
ssh pi@raspberry.local
```

you are now "logging into" the pi, so the password requested here is the one we setup earlier for the pi. enter that.

You should now see your terminal username change from your laptop's local user to something like: 

``` shell
pi@raspberry~$.
```

To exit out of the pi type:

``` shell
exit
```

Hit enter.

---
## Tmux installation

Tmux is a terminal multiplexer, for collective editing in a terminal. Again, we are going to install this on the server (the pi). To use it you must be SSHed into the server as above  [[01-Set up Pi for local Network Access#Security via SSH Keys]].

So, assuming you are SSHed into your server, follow these steps. We are going to install TMUX as sudo:

```
sudo su
```

You might be prompted to put in a password.

To install:

``` shell
apt-get install tmux
```

To create a new tmux session sign into the user under which you would like the session to be registered.

A tmux session is a shared terminal session where all those connected can input and execute commands synchronously. Below name your session, by replacing `[name]` with a name of your choice :

``` shell
tmux new -s [name]  
```

example:` tmux new -s mySession

To join the session when on a server use the following command replacing `[name]` with the name of the session:

``` shell
tmux a -t [name] 
```

example: `tmux a -t mySession`

Note: Everyone in a Tmux session is acting as the same user. However we can create split screens and multiple panes within Tmux so different people can work on different things. 

Reference and troubleshooting: https://www.howtogeek.com/devops/how-to-get-started-and-use-tmux/


[TMUX cheatsheet](https://tmuxcheatsheet.com/)


---

## Installing Node using Node Version Manager (NVM)

 NOTE: Best to install node per user, so make sure you are not in sudo mode. 

Install nvm:

```
wget -qO- [https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh](https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh) | bash
```

**Read the output,** and follow the prompt given by nvm to run the selection of commands to add nvm to your bash profile to start using nvm without closing the terminal. 


Install latest version of node + npm by doing:

```
nvm install node
```

!!!!! Debugging: IT DIDN'T RECOGNISE nvm !!!!

Look above to the bold **Read the output** line...

You can install different versions of node, e.g.
```
nvm install 12.3.0
```

Use that version:
```
nvm use 12.3.0
```

List all node versions:
```
nvm ls
```

- [NVM git + docs](https://github.com/nvm-sh/nvm)
- an nvm [cheatsheet](https://cybertooth.io/blog/2017/07/13/nvm-cheat-sheet.html)


--

### Use github to get code onto your pi


Get your github repository of choice as a zip onto your pi.

1. Open GitHub repository.
    
2. Right click the **_Download Zip_** link, and copy the URL.
    
3. Use that copied URL with `wget` in terminal:
    

Don't just copy and paste! You will need to replace the bits between the <> brackets.

Wget command:
```
wget <your url>

```

Unzip the folder that was pulled down:
```
unzip <filename.zip>
```

Change directory into the folder that you unzipped, e.g.
```
cd <your-folder>
```


NOTE: You could also install git and clone the repository if you plan to do any development on the Pi using Nano/Vim (command line text editors).

  Follow the Linux instructions here for installing git: [https://github.com/git-guides/install-git](https://github.com/git-guides/install-git)

--

### Node.js Reminder

Navigate to the folder you see with the package.json:

```shell
npm install
```

```shell
node app.js
```

Next in your browser go to IP or hostname + the port number

e.g.
raspberrypi.local:3000

run `ctrl+c` to stop the server

--

### How to leave your node server running?

You can quickly do this useing nohup:

```
nohup node app.js
```

You can then close the terminal, exit out of the pi via ssh and it should still be running. If you want to stop the process later in the terminal:

```
ps -ef | grep "node"
```

EXAMPLE OUTPUT:

goldpi      5406    2862  0 15:59 pts/1    00:00:03 **node** app.js

Where 5406 is the Process ID (PID)

more on ps [here](https://www.baeldung.com/linux/ps-command)

Use the Process ID to kill that one process:

```
sudo kill <PID>
```

e.g. from above output `sudo kill 5406`


--
###  Using Systemctl 

We can use [systemd](https://en.wikipedia.org/wiki/Systemd) the Linux system and service manager to automate things. We can use systemctl to manage systemd services. 


Create a new system service file by doing this command:

```
nano /etc/systemd/system/static-node-app.service
```

In nano text editor add these contents:

```service
[Unit]
Description=My Node.js App
After=network.target
[Service]
ExecStart=/root/.nvm/versions/node/v21.6.1/bin/node /home/pi/node-static-server-main/app.js
WorkingDirectory=/home/<user>/<your-node-project>
Restart=on-failure
Group=sudo
User=root
[Install]
WantedBy=multi-user.target  

```

NOTE: test the node path in ExecStart... can we use npm?? 