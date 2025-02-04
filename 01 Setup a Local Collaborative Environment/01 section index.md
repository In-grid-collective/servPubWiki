
### Description of this section

 ![[Pasted image 20231108164245.png]]

**Section 01** will cover the steps necessary for setting up a local collaborative environment. In practical terms, this means setting up a Raspberry pi which a number of users can access to work together.

The section covers:

[[01.1 Hardware and OS]] 
[[01.2 Creating Users]]
[[01.3 SSH]]
[[01.4 TMUX]]

### Prerequisites

The following are necessary for following on with these steps.

- Raspberry pi + peripherals: HDMI screen, keyboard, mouse etc. We're using a Raspberry pi.
`You don't necessarily need to use a pi to create a collaborative environment, you could use another type of computer. To understand more about why we chose to use a pi, you can find our notes here`[Chapter 2a: Server Issues: Platform Infrastructure](https://wiki4print.servpub.net/index.php?title=Chapter_2a:_Server_Issues:_Platform_Infrastructure)`
- Pi OS booted: The Rosa Server Guide recommends [Armbian OS](https://www.armbian.com/rpi4b/)
- Knowledge of terminal/bash 
`For an intro to basic terminal/bash commands see` [[05 Appendix/NA-Terminal Unix Commands Cheat Sheet|NA-Terminal Unix Commands Cheat Sheet]]
- Have SSH installed on laptop. Most OS have it by default now, if not then manually install following the steps under [[01.3 SSH]]

##### Note on terms:

Throughout these docs we'll be referring to the server and the client:

- Server = Raspberry Pi/SBC (Single board computer)
- Client = your laptop

## Users and collective work

If you are following these docs as a guide we are about to begin setting up a space for collective work. When setting up a machine that is going to be accessed and potentially also maintained by several people we should consider how best to approach this. This is both a question of governance (to what degree different people can have access), but also transparency and accountability (who was online when something happened). 

At some point in developing the collaborative environment, we should discuss and choose a user and access structure. Whatever structure is decided upon, we should decide how to communicate between sysadmins, how to keep records and how to handover work etc.

As we will come to see in more detail in [[01.3 SSH]] when we enter a machine via ssh, we specify that we are entering into a username's account on that machine. That means that everyone ssh-ed into that machine is acting as the same user. 

`You will see a username that is something like pi@10.0.10.x. "pi" in "pi@10.0.10.x" is the name of the user on that pi. `

This means that in some instances you will be acting on behalf of others. For this reason it's important to establish consistent protocols for co-working.

During a working session we noted that there is fine line between transparency and surveillance. However, we acknowledge the need for accountability to these concerns and discuss how we can approach and support each others mistakes when things go wrong (which they will). This is another reason to take the time to establish how to work comfortably as a group early in the process. 

During our first working session [[Meeting Notes_05062023]]] Luke and Mara both shared examples from their own experience and collective work with Systerserver and Varia. Both collectives made different user accounts for each person who needed access but not everyone needed sudo access.  When accessing Jean, the systerserver box (see [[02-Network Anatomy + Local Setup on Pi]] ) we made users for everyone who needed access and we made everyone a sudo user.

Reference for how to create users and make them sudo can be found here [[01.2 Creating Users]]

## Names

During the process of setting up your collaborative environment, we will need to give certain elements names. For example, we will come to refer to a username on the server (Servpub01) and the server's hostname (pubdoc). A hostname is a unique, human-readable name that identifies the server on a network.

When creating systems you should consider what you would like your naming conventions for users and hostnames. The conventions should be legible and easy to remember and to discern from each-other. You might also want to make them significant to you and your project. 



- [ ] Should the users and collective work be here?
- [x] Make appendix section, make intro section
- [ ] General notes, more guiding language
- [x] Terminal bash in the appendix
- [ ] Find link to the rosa server manual
- [ ] Can we link to our platform infra workshop here?