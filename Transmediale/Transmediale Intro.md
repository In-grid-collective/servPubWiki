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


<!-- .slide: class="bg" -->


---
## ServPub 
...is a platform for research and practice on experimental decentralised computational publishing, to reflect collectively on affective infrastructures, minor tech and autonomous networks within, and beyond, institutional constraints.

ServPub is run by artists, coders, activists, collectives, scholars and researchers using FLOSS, who share feminist values and practices. We aim to build shared knowledge and resources which operate at small scale and as part of grassroots community networks to explore alternatives. 


---
# Collaborators

<br/>
#### Organising bodies: 
Winnie Soon, [CCI](https://www.arts.ac.uk/creative-computing-institute), UaL.
<br/>
Geoff Cox, [CSNI](https://www.centreforthestudyof.net/?page_id=756) at London South Bank Uni.
<br/>
Christian Ulrik Andersen, [DARC](https://darc.au.dk) & [Shape](https://shape.au.dk) at Aarhus Uni

---
#### Participating Collectives: 
[In-Grid](https://www.in-grid.io/) Project Implementation w/
<br/>
[Systerserver](https://systerserver.net) Mara and Ooooo: Infrastructure/VPN 
<br/>
[Varia](https://cc.vvvvvvaria.org) Luke: First workshop guide - local setup 
<br/>
[CC](https://cc.vvvvvvaria.org/) Manetta and Simon: wiki-to-print 

---

![[servpubProjectMap.png]]

---
# In-Grid 


Batool/ Becky/ George/ Katie/ Sunni

+ 
![[Batool.png|110]] ![[Becky.png|110]]![[George.png|110]]![[Katie.png|110]]![[Sunni.png|110]]
 
_In-grid is a group of many - these are the members who are actively involved in this project_

---

# Resources:
| _From CCI_ | _From In-grid_ | _From Systerserver_ |
| ---- | ---- | ---- |
| x2 raspberry pies & peripherals | Screens & peripherals | Public IP / Server (Jean) |
| SD cards & 4G dongle | Space + electricity | Rosa Manual - (Varia, HYPHA, LURK, esc, Feminist Hack Meetings, Constant) |


---
# Resources:

## Time!
A resource we all had to share, split and make


---

## Timeline

<br/>

- May - Public workshop 1 (CCI), project launch
- August - Semi-public workshop -  tinc & nginx
- November - Public workshop (LSBU) wiki node joins VPN
- December - wiki-to-print becomes wiki4print (installation)
- Jan - Transmediale, content/form

<br/>

_Between these public facing events there were many meetings, co-working sessions, discussions and collective debugging._

---

## Network Anatomy 

Here is an overview of servpub to introduce different elements of internet infrastructure and associated terms.


---



![[slideAsset 6.png]]

---

![[piBerlin.jpeg]]

---

![[slideAsset 2.png]]


---



| Term     | Example | Description |
| ----------- | -----------  | ----------- |
| IP Address   |  192.0.2.1 | IP (Internet Protocol) addresses work a bit like a street address in that they allow devices to find each other on a network.      |
| Static IP |  89.106.208.4 | Unlike standard dynamic IPs, a static IP is assigned to a device and it remains the same. |
| Domain Name |  servpub.net | IP addresses can be mapped to a name/text via a Domain Name System (DNS) format.   |
| Ports | 80 | Port 80 is the default port number assigned to internet communication protocol, Hypertext Transfer Protocol (HTTP). Ports are part of the TCP transport layer.|



---

![[slideAsset 7.png]]

---



| Term | Example | Description |
| ---- | ---- | ---- |
| Tinc |  | VPN software |
| Subnet IP Addresses | 10.10.12.* | IPs in this range are reserved for private networks |


---


![[ifconfigberlin.jpeg]]


---

![[slideAsset 4.png]]

---


![[slideAsset 5.png]]

---


| Term     |  Description |
| ----------- | ----------- |
| Static Server |  A Server for serving up files like .html .jpeg .css .js etc  |
| Proxy Server | A proxy server is a go‑between or intermediary server that forwards requests for content from multiple clients to different servers|
| Reverse Proxy Server  |  A **reverse proxy server** is a type of proxy server that typically sits behind the firewall in a private network and directs client requests to the appropriate backend server    |
| Nginx |  **Nginx** is open source software for web serving |

---

## Collective working

Part of our process was working collectively on the command line, using a software to enable this:

![[Pasted image 20231123170353.png]]

We also spent many hours on _**etherpads**_ jotting notes and chatting through ideas, meeting in person and online using platforms like _**jitsi**_.



---

## Documentation

Another element of this work is the creation of cowritten documentation, intended to allow others recreate and remix this work. It is available at:
<br/>

https://git.systerserver.net/queer/networks



---

## Future...
