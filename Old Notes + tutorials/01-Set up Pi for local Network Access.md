_this guide is a work in progress compiled from research on setting up local servers, it will change to reflect the actual steps in-grid takes as we work on this project_
# Local machine remote access

![[Local IP.png]]
## Prerequisites

- Raspberry pi + peripherals: HDMI screen, keyboard, mouse etc. 
- Pi OS booted: The Rosa Server Guide recommends [Armbian OS](https://www.armbian.com/rpi4b/)
- Knowledge of terminal/bash
- have SSH installed on laptop (most OS have it by default now, if not then manually install, below)

##### Note on terms:
- Server = the SBC/Pi
- Client = your laptop

## Armbian 

After flashing the Armbian disk image on the SD card, plug the Pi in and then just follow the prompts on screen.

If for any reason (like a power cut or accidental un-plugging) you have to enter the Armbian default username and password before you can boot the OS. It is usually:

- blank user name
- pw: 1234

Trouble shooting from this link. https://raspberrytips.com/armbian-on-raspberry-pi/

## Installing SSH server & client:

*You might need to install a package for each, still need to verify this on windows machines:

- the server package is installed on the SBC:

``` shell
sudo apt install -y openssh-server
```

- and the client package is installed on your laptop:

``` shell
sudo apt install -y openssh-client
```

If you are installing ssh for windows or mac refer to [[NA- Installing SSH on other devices.md.md|NA- Installing SSH on other devices]]
## Change Default PW:

Defaults are 
- Username: Pi
- PW: raspberry

Note: If you do not know the username, you can use this command in the pi terminal and it will display it.
``` bash
whoami
```


Note: There is no need to know what the current password is in order to change it.

Change password by: 

```shell
sudo passwd pi
```
It will respond:
``` shell
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

Do take note of the password.
## Update & upgrade system repos:
On the server, execute the two follow commands:
```shell
sudo apt-get update
```

and
```shell
sudo apt-get upgrade
```

##  Enabling SSH 
 
connection:
On the the server, execute: 

```shell
sudo raspi-config
```

Or, go to the "config" window through the Pi's interface. 
	
Look for _Interfacing Options_, find SSH and enable it then click OK to exit and finish.

Note: While you are here it is also worthwhile to enable VNC as it will save you the pain in the ass of having to use a keyboard and mouse attached to the pi. 
	
Documentation on remote access for pi [here](https://translate.google.com/website?sl=auto&tl=en&hl=en&u=https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) 

## SSH requirements:
- being on the same internet router as the pi

Note: If you are moving around, get a dongle, tether form your phone data package or "share wifi" form your laptop.

- IP address of the pi: 

one way to do this is, through is this command line on the pi:
```shell
hostname -I
```

or, scan the network from the client (laptop) using: 

```shell
nmap
```

nmap requires that you know the IP address of the network your on. You can easily find this from your network settings on your laptop. The first three numbers of your network IP will be the same on the pi, but you have to tell the scanner to traverse all possible options for the last number. 

Assuming that your network IP is 192.168.0.0, This is the syntax to do that:

``` shell
nmap 192.168.0.0/24
```

You can also use a network scanning app (on a phone) and it should come up. 

Pro tip: don't do it in a building with over 200 devices like I did.

## Connecting from a client:
now that we have the IP address, any computer can request to SSH into the Pi through these commands:
``` shell
ssh pi@<pi ip>
```
eg.
``` shell
ssh pi@100.10.1.90
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

Hit enter. You will need to have exited the pi before setting up your ssh key. 

## Security via SSH Keys
SSH Keys are user specific and are used in addition to a shared login password. To make this method of access truly secure we will need to eventually disable password-only login. 
	
The shared password is the Lock, and the private password is the Key. Keys are non-transferrable and have to be generated per user. 

To generate these, execute this command on your laptop: 

``` shell
ssh-keygen -t rsa
```

fill in the information requested (note: most of it is optional so you can leave it blank) and set a password (not optional).

the shared key is the:

`id_rsa.pub`

and the private Key is the:

`id_rsa` 

Make note of the path to your id_rsa.pub, we'll need it in the next step.

The public, Lock, password now needs to be copied over to the raspberry pi. Do this via this command (replacing the first bracket with the path to the id_rsa.pub, and the second to the Pis IP address, which we found earlier):

``` shell
ssh-copy-id -i <path to id_rsa.pub> pi@<ip address of pi>
```

Remember that the pi might now have a different name thats not "pi" so you should address it accordingly (don't deadname your pis). 

If you are using windows 10 refer to this post for an alternative to ssh-copy-id https://chrisjhart.com/Windows-10-ssh-copy-id/

Because the Key system is not yet in place, it will ask us for the pi password we setup at the beginning of config, so insert that here and hit enter.
	
This should copy over the key. 
	
To test that is works, try to SSH into the pi again. This time it should ask for the Lock and your private Key. The command to SSH is still the same: 

``` shell
ssh pi@<ip of pi>
```

SSH key trouble shooting or if you want to see the contents of the authorized keys file on the pi you can cd into the invisible ssh folder to list (ls) out the files. Doing cat < filename > will log out the contents of the file:

``` shell
cd /.ssh
ls
cat authorized_key
```

## More than one SSH KEY

If you machine already has an SSH key, or you'd like to generate more for different servers you can do that by customising the name of the file of the public key. So instead of "id_rsa.pub" the file name can another name. 

Ideally this name would match the server you want to use that key for. eg: "id_rsa_server_name.pub". To do this use only the first half of the keygen command:

``` shell
ssh-keygen
```

This will tell you that it is generating a key, but will then also ask you to specify the file name:

``` shell
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/Admin/.ssh/id_rsa):
```
the second line already gives you the path, copy/paste that and add to it the file name. Here we are giving it the name "id_rsa_server_name":

``` shell
/Users/Admin/.ssh/id_rsa_server_name
```

The next steps are the same: enter a passphrase and you will have the path of your key. 

You can navigate through your finder to the /.ssh file inside your user folder. You will see all the public key files there. To share your public key, you still need to copy it over to the server using the same command as above:

``` shell
ssh-copy-id -i <path/to/id_rsa.pub> user@<ip address of pi>
```

## SSHing while having multiple keys

When SSHing into the server from a machine that has several keys, you have to specify which key you want to use to ssh. To do this, you should include the directory path to your PRIVATE key:

``` shell
ssh -i <path/id_rsa_anotherkey.pub> <user>@<server address>
```
You should now be prompted for your personal password. 

## Troubleshooting SSH

https://betterprogramming.pub/how-to-set-up-multiple-ssh-keys-ae6688f76570

https://homebrewserver.club/demystifying-ssh.html#troubleshooting-ssh 

## Disabling password access

_Only do this after everyone who need to access the pi has uploaded their Keys!_

note: 
We need to check if we should do this at all, especially if we need some form of "public" or non-member access.

Access the config file by:

``` shell
sudo nano -l /etc/ssh/sshd_config
```

And in uncomment 'PasswordAuthentication' on line 57 ish exitand set to 'no', then save and exit:

``` config
PasswordAuthentication no
```

``` comands
(ctrl + 0)
(Ctrl + X)
```

Then restart the SSH connection:

``` shell
sudo service ssh restart
```

## Forgotten your ssh pass phrase???? 
(Like George did . . . )

ooooops, that was silly. Maybe get a password manager (like George did after).

### Find a friend who has access. 

Find someone who can get access to the server and has **sudo** permissions.

Then ask them if they can help for a minute. 

### Create a new set of ssh keys

Maybe see [[Documentation_01-Local Config#Security via SSH Keys]] for more info, but you can also just follow the steps below.

Create a new key:

``` shell
ssh-keygen -t rsa
```

### Copy over the new key

Send the `id_rsa.pub` (however you named it) file to your friend with access, do not send the private key (a.k.a. `id_rsa`) . They will then copy over the new public key to the server.  

They will need to log in via ssh in:



# Tmux installation

Tmux is a terminal multiplexer, for collective editing in a terminal. Again, we are going to install this on the server (the pi). To use it you must be SSHed into the server as above  [[01-Set up Pi for local Network Access.md.md#Security via SSH Keys]].

So, assuming you are SSHed into your server, follow these steps.

To install:

``` shell
apt-get install tmux
```

To create a new tmux session sign into the user under which you would like the session to be registered (in our case sudo). We would do this by executing the command:

``` shell
sudo su
```

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

