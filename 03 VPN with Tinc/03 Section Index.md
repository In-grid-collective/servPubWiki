# Tinc

Tinc is a Virtual Private Network (VPN) daemon that uses tunnelling and encryption to create a secure private network between hosts on the Internet. Find out [more](https://www.tinc-vpn.org/) from their site.

- [ ] what is a vpn! 

>[!error] Deprecated
>Tinc is becoming deprecated . . . . this means these docs aren't very maintainable as this software will become insecure and start to break. Why have them here? well it is partly to show a key resource in the making of this project, and one that has been shared and reused in different contexts. It is also to show the history of why we are using these software, which you can read under [[03 Section Index#Why Tinc?|Why Tinc?]]


## Why Tinc?

We are using tinc because it is inherited from the history of projects that we are working with. This setup pulls from the original work of [XPub](https://xpub.nl/) and their [HUB](https://pzwiki.wdka.nl/mediadesign/HUB) project, which used it to form experimental server space for their students which could get passed institutional firewalls securely and let devices roam. Similarly we used the setup to form an experimental network of servers to form a collective publishing infrastructure. 

You can read more on the history at the bottom of [this page](https://circulations.constantvzw.org/about.html) under the heading _Radical Referencing_.

Other groups have also built from this configuration. Below is a list of other resources and docs on how to setup tinc that we have worked from/with:
- XPub docs - [Tinc VPN install](https://pzwiki.wdka.nl/mediadesign/Tinc)
- Lurk docs - [Tinc VPN install](https://things.bleu255.com/runyourown/VPN_with_Tinc)
- Psaroskala zines - [Making a private server ambulant](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf)

## To Install!

To install Tinc, you need at least two machines:
1. A main server/hub with an open IP address
2. Node(s) to connect to it

You need to install Tinc on all devices, but the configuration on the hub and the node will be slightly different. 

Below is the order of the Install pages:
- [[03.1 Tinc Install]] 
	- You need to install tinc for all nodes, both server and client(s).
- [[03.2 Tinc server side prep]] 
	- Make these edits just to the serve/hub
- [[03.3 Tinc Client Join]]
	- Follow this to add nodes to the network.
- [[03.4 Tinc Automation]]
	- setup on all devices you want the VPN to automatically run.

If you want to install on Windows, it may be better to run tinc from a Ubuntu console [like this](https://www.microsoft.com/store/productId/9PDXGNCFSCZV?ocid=pdpshare)
# todos

- [ ] maybe more info on what a vpn is
- [x] more conceptual critique of tinc and how we are working with it.
- [x] describe clearer the different routes for comp and server.