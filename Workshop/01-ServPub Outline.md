---
theme: moon
---

<header>
<link rel="stylesheet" media="screen" href="https://fontlibrary.org//face/generale-station" type="text/css"/>
</header>

<style>
    
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
h4{
	font-family : "GeneraleStationRegular"!important;
        font-size: 0.8em!important;
        
}
	code{
	     padding: 0.5em 1em!important;
	}

.slide-background{
background: rgb(79,9,121); background: linear-gradient(180deg, rgba(79,9,121,1) 4%, rgba(52,1,45,1) 100%)!important;
}
</style>

![[servpubProjectMap.png]]
---
ServPub is an experimental platform for research and practice on experimental and computational publishing, to reflect collectively on affective infrastructures, minor tech and autonomous networks within, and beyond, institutional constraints.

ServPub will be run by artists, coders, activists, collectives, scholars, researchers using FOSS, who share feminist values and practices. We aim to build shared knowledge and resources which operate at small scale and as part of grassroots community networks to explore alternatives. Participating communities/institutions include CSNI at LSBU, Creative Computing Institute at UAL, SHAPE at Aarhus University, In-grid, Systerserver, and Varia.

---
# Timeline
```timeline-labeled
[line-4, body-4]
Date: 26 May 2023
Title: Public Workshop 1 (CCI)
Content: [[Meeting Notes_Workshop UaL CCI]]

Date: 19 July 2023
Title: Working Session 1 (online)
Content: installa nginX & tinc

Date: 4 August 2023
Title: Working Session 2 (CCI)
Content: config and get online

Date: 6 October 2023
Title: Big regroup meeting (online)
Content: Catch-up & discuss workshop

Date: Oct -- Nov 2023
Title: Several internal working sessions
Content: Documentations and workshop planning

Date: 24 Novemebr 2023
Title: Public Workshop 2 (LSBU)
Content:

Date: 8 December 2023
Title: wiki2print install
Content: session with Varia 

Date: xx January 2024
Title: Transmediale Workshop
Content: ??
```

--
# Timeline
### 26 May 2023: Public Workshop 1 (CCI / Hybrid)
- InGrid to configure local remote access to the pies.
- Testing Wifi login at UaL/CCI campus (14 May)
- Discussion with Mara & Luke
- Generally understanding things
--
# Timeline
### 19 July 2023: Working Session 1 (online)
- ssh-ing
- hostnames
- tmux
- NginX 
- install tinc on pubdoc (Linux)
--
# Timeline
### 4 Aug 2023: Working Session 2 (CCI)
- tinc on sysadmin machines (mac & win)
- configure tinc
- configure nginX
- servpub.net online
--
# Timeline
### 6 Oct 2023:
- Big regroup meeting (online)

### Oct - Nov 2023: Several working sessions
- SSL & HTML Certificates
- Populating Documentation 
- planning workshop
--
# Timeline - Future
### 8 Dec 2023: Working Session x Varia
- Wiki2print install on wiki2Print pi
### January 2024
-  Transmediale Vaaria Workshop 
---
# Purpose
### Using, hosting, publishing, sharing
possibilities of what to host and do with the server space

prototyping space for networking projects

--
# Ideas to Host
- Docs
- WriteFreely
- Etherpad (synced with docs?)

---
# Collaborators
#### Organising bodies: 
Winnie Soon, [CCI](https://www.arts.ac.uk/creative-computing-institute), UaL.
Geoff Cox, [CSNI](https://www.centreforthestudyof.net/?page_id=756) at London South Bank Uni.
Christian Ulrik Andersen, [DARC](https://darc.au.dk) & [Shape](https://shape.au.dk) at Aarhus Uni

#### Participating Collectives: 
[In-Grid](https://www.in-grid.io/) (Batool, George, Sunni, Becky, Katie): Project Implementation
[Systerserver](https://systerserver.net) (Mara and Ooooo) workshop guide & 
[Varria](https://cc.vvvvvvaria.org) Luke: First workshop guide - local setup / (Manetta and Simon): wiki-to-print 

---
# Resources:
| _From CCI_ | _From In-grid_ | _From Systerserver_ |
| ----------- | ----------- | ----------- |
| x2 raspberry pies & peripherals | Screens & peripherals| Public IP |
| SD cards & 4G dongle| space| Rosa Manual |

> [!error] Electricity
> Is a resource that we happen to have access to at no extra cost at our studio buildings
<!-- element style="width:70%"-->

--
# Resources:

## Time!
A resource we had to share, split and make

---
# Agenda:
Feminist Methodologies - Systerserver

Setup time - Downloading some tools

Hands-on: Network Anatomy - Becky & Katie

Break

Demo: VPN & Reverse Proxy - George & Batool

Break

Feedback: Documentation & Discussion - Notes
