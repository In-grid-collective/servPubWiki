
_This guide is a work in progress compiled from research on setting up local servers, it will change to reflect the actual steps in-grid takes as we work on this project_

### description of this section

 ![[Pasted image 20231108164245.png]]
### Prerequisites
- Raspberry pi + peripherals: HDMI screen, keyboard, mouse etc. 
- Pi OS booted: The Rosa Server Guide recommends [Armbian OS](https://www.armbian.com/rpi4b/)
- Knowledge of terminal/bash 
- have SSH installed on laptop (most OS have it by default now, if not then manually install, below)
##### Note on terms:
- Server = the SBC/Pi
- Client = your laptop
## Users and collective work

As we have seen when we ssh into a machine we specify that we are entering into a username's account on that machine. That means that everyone ssh-ed into that machine is acting as the same user. The "pi" in "pi@10.0.10.x" is the name of the user on that pi. 

When setting up a machine that is going to be accessed and potentially also maintained by several people we should consider the best approach to this. 

This is both a question of governance (to what degree different people can have access), but also transparency and accountability (who was online when something happened). 

Note: during a working session we've noted the fine line between transparency and surveillance, however, we should probably also add the need for accountability to these concerns and discuss how we can approach and support each others mistakes when things go wrong. 

Note 2: no matter what user and access structure we choose, we should discuss how to communicate between sysadmins and how to handover work, etc. 

Luke and Mara both shared examples from their own experience and collective work with Systerserver and Varia. Both collectives made different user accounts for each person who needed access but not everyone needed sudo access.  For Jean, we made users for everyone who needed access and we made everyone a sudo user.

Reference for how to create users and make them sudo can be found here (link to systerservers steps) **<<<<<<<<<MAYBE JUST add these steps in as well? have you got the link?**
## Names

So far we have run into two names, the username on the server and the hostname. These are Servpub01 and pubdoc respectively. 


* [ ] Should we have a more general overview here?
- [ ] Should the users and collective work be here?
- [ ] Make appendix section, make intro section
- [ ] General notes, more guiding language
- [ ] Terminal bash in the appendix