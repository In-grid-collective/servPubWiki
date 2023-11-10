---
type: Notes
alias: workshop notes servpub
created: Wednesday 28th June 2023 21:45
project: Workshop Servpub
---
_these notes are still unedited_
# Notes:
____
# Intros

publishing practices inside Minor Tech via wiki-to-print and 

Questions:
- was there a model for collectives that we looked to? 
- how do we organise?

Luke intro:
- the decision making of the people using it and are actively engaged will/should be the ones who it will work for best. 

Pi name is still rpi4b. The main user is servpub01.

Access point (which we call the router) is the bridge between the address of the server and the client (personal machines).

We can 

Joining the Tmux session: tmux: servsession ("sudo -su root tmux attach")

# Local IP:

You 

# Change the hostname:

"servpub01" VS "rpi4b"

Host Key is the address of the server that will be shown when you first SSH into the server. this no. can be saved physically somewhere else to tell other who are SSHing into the server what key to expect when they try to SSH. 

Admin on the server-side can regenerate these keys. 

# SSH options:

General pwd login, can be brute-forced into if people can keep trying to generate passwords. the way to make it more secure, is to use the SSH key method. 

# Users:

## Systerserver Recc: 
every users have their own user account and their own SSH keys. 

## Rosa: 
had a gues user called "friend" for workshops and guest access. and their admin has different users and some of them had root access.

## Varia: 
The serves was maitnatined by a subset of the members and other members would ask for ask access and they would be given an account, then they can ask further to become the root user. 

The power dynamics and accessibility to experiment whens ome members do not have access. especially to things that are not publicly accessible. 

What happens to members who leave?
Deletions: when people leave or pause memberships you *Can* delete users. 

Requirements for getting root access:
- must gen a SSH key
- must put a password in place
- must have nother passwrod for root / sudo power

After everyone had individual user accounts, they moves towards some group or shared accounts for group working sessions. 

Sudo/root is also a committmenet to resposibily and care of the system/server.

Eg from Varia: 
- for the etherpad: users had to keep restarting the pad in orde r for it to stay up. this requireed sudo permissions so everyone needed that access. (worked well)

- installing pything packages gets confusing when someone installs at sudo pip level serves just pip levels. (things broke)

Decide on the challenge of comms for troubleshooting. 

With network comms between servers and sysadmins on every server, how to communicate will be very important. 

Governance question: to what degree can people have access

Zellij; is Tmux but it allows you to be multipul curors.

# Bash scripting:

recc from luke is to do this at the last stage after everything is set up. 

Decisions to be made: how often to do which check: updates, upgrades, backup (?)etc. 

This should be put in place after we do this manually, then take note of the tedious tasks that we would like to automate. 

Maintaining the scripts requires knowing how to write them and how they break. Familiarity with that process is important even at different levels of comfor

## avoid for:
- working with Json / YAML and other data structures
- flags and options

Tools to help with that: gum.
ref: [https://vvvvvvaria.org/~decentral1se/tools.html](https://vvvvvvaria.org/~decentral1se/tools.html)  

This will become apparent when we have multiple servers and/or batch tasks. Luke will try to have access to their update script to share with us (they are open source but theyre in a private server).

## Backups
- borg - python (systerserver)
- rustic - 

can/should have a dedicated pi for backups. Can run the backups at a certain time. The scheduling can happen with the system unit.

the script calls the tool and finetunes its options per how we want to do it. 

# Install reccommendations:

## Apache vs. NginX

depending on which language we want to work with mainly. PHP is good with nginx.

Apache: modules. shorter. 
nginX: less. longer. more involved.

Systerserver already has Apache for a long time. 

## uncomplicated firewall

ufw is an ip tables tool - look for guides.

## full list of tools:

[https://vvvvvvaria.org/~decentral1se/tools.html](https://vvvvvvaria.org/~decentral1se/tools.html)   

# Hostnames:

our hostname is currently: rpi4

so we have our IP address, the name we have on our network and we also have our hostname which is the human frindly version of the IP address. 

For a local webserver we should use the hostname and not the IP. We can change it by editing the file inside etc/hostname.

vi/etc/hostname

We should decide on the naming system of the pi and several other pis. eg. the servpi the wikipi the librarypi. Descriptive mode. Or choose other names or naming inspirations. 

G: we should cnc our own heat sinks lol

Affective aspect of naming!
Mapping where all the servers are. 

	hostnamectl hostname <newname>

we chose pubdoc, so:

	hostnamectl hostname pubdoc

	hostnamectl status

will show the status and cofirm the the name of the hostname.

# Networking online

Zine by systerserver: 

# Static IP 

expand the SD card of the server first of all. or get a portable harddrive for storage.

the MAC address is always the same, for RPi4 B they start with dc:a6:32

You can go in a browser into the root IP to see the config

192.168.1.1 is the root IP in this case. literally just paste this address into the browser. 

We need to see all the IP addresses that the access point (the dongle) can assign (why?) and pick an address. Probably is assigned between 2 and 253

do 

	arp-scan -l 

to list the IPs that ARE taken. 

Note: this specific access point cannot do port forwarding. You have to be able to forward specific ports that you want to open to have this server accessible. 

- **Open the configuration file with:**

`sudo nano /etc/dhcpcd.conf`

actually no the previous step wont work for armbian. We need to search for how to open the config file for Armbian. 

local host must be changed on the list of hosts as well as

etc/hosts

etc/nginx/sites-available/default 

nginx -t  (for troubleshooting configs)

now the name of the host (pubdoc) can also be seen on a browser. so via: 

	pubdoc.local 

should show the same page as

Reboot the machine to get the local domain. 

IP

We need a public static IP (to buy)

NOTE: When we register a domain name, its god to be collective and separate from other individual domain name accounts. 

The certificate will be installed on the public server, for now that is systerserver. 

Note: local networs always start with 192 IP addresses. 

VPN is a workaround for the fact that the Pi doesnt have a public IP. It is overcoming the restriction of firewalls. 

Layers:

Public IP
.
.
access point / dongle
.
.
local IP (pubdoc)

you need an access point through the public IP layer to access the pi from anywhere on any network. a VPN will do that. VPN will connect us to the public IP by assigning a Private IP (different to a local IP)

TINC will act as a VPN. TINC  doesn not have the master server with multiple clients herirarchy. instead, everything is a node, similar to a p2p relation. 

But still the public server is a big dependancy, even if it is not explicitly structured as a heirarchy. 

Systerserver will act as out public static IP for the next 2 months. Once we set up through them we should move and private IP and remap from the public domain (DNS).

To SSH remotely (actually remote)

We need to be part of the VPN "club", which happens through an invite/registration and give it your personal machine name. And you have to install tinc. and open specific ports on the public server. 

Once you run tinc on the server, you set up a private IP and this happens only once. 

You can do it without being part of the VPN using a proxy. you dont need to install tinc but you need to give your SSH key to the public server. and it will forward you to the pi. This is the structure that varia uses for "freind" accounts.

The traffic of the VPN server is logged and can technically be published. Providers usually comply with requests to publish their logs but usually the logs are wiped periodiclaly. 

https://psaroskalazines.gr/pdf/ATNOFS-screen.pdf 

run my Mur.at

## how to sustain the project:
- attach to an institution
- public funding
- services provided

for now, its okay to circumvent the academic system but for the future it would be good to go through the university IT. to start with we can just say, here is the man address of this specific pi and we want to open port 80. 

speaking the language of techincal competence allows for some kind of solidarity, including sharing/expressing their hatred for (which they probably secretly hold) for Microsoft services.

Ripe interent network NGO - sys applied for VPN education manuals and guides. 

Negotiate to have the public server at an institution, including arebyte as an option. or talk to NeoN, Dundee.

# Staying in touch

ways of being in touch thats not sending each other emeil. pubnix. a social digital space within the server itself. 

Server Pub 

servpub02 : wiki to print or wikitoservpub.net

