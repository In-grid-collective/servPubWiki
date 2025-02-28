
The next step is to install and configure the software we need to make our pi machine into a web server. We will be installing and using the open source software [NGINX](https://www.nginx.com/) to so this.

This section will take us through all steps needed to install, configure and implement some automated jobs in the open source version of NGINX. There are  [many other ways to install NGINX](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) and several other version of it that can be used, but this guide outlines the methods we used for the ServPub project using the open source version of NGINX.

> [!NOTE] What is a Server
> A server is a computer that provides information to other computers called "clients" on a network. Servers can provide various functionalities, called "services", such as sharing data or resources among multiple clients. 
> In our case, we are using a server to "serve" a simple website to the network locally and on the public internet. NGINX does this by handling the static content delivery, of our website files including HTML, CSS and images. We will setup these in the coming section.

NGINX has many other uses, and in the [[04.1 NginX for Reverse Proxy|Reverse Proxy]] section of this guide, we will use it again to setup different parts of the network.
## Section Contents:
[[02.1 NginX]]
- Install and access files
[[02.2 Static NginX]]
- Setting up HTML files and NGINX config
[[02.3 Systemctl and autotmation]]
- Creating some automated tasks to restart on reboot

- [x] This intro needs re-wording to make clear what we're doing here.
- [ ] this is post-rewording. check now.
- [x] too many links out? -keep, are helpful.
- [x] linear stream vs linking to further sections?
- [ ] We can put a link to the certbot stuff here IF you are setting up a static server that is connected to a static IP online, and not doing a VPN. 