# Analog Interlude
_you do not need to be at your machine for this bit_

So now we know how to connect to a server machine that is on the same network as our personal machine. The next "logical" step would be to figure out how to do this _across_ networks but we won't do that yet. 

We have to take an analog interlude to decide on two key things that will make the future steps a lot easier and more clear. Even if we don't make the final decision about this now, now is a good time to consider them and we can come back to them later.
## Users and collective work

As we have seen when we ssh into a machine we specify that we are entering into a username's account on that machine and everyone ssh-ed into that machine is acting as the same user. 

Servpbu01 is the user for pubdoc. The "pi" in "pi@10.0.10.x" is the user on that pi. 

When setting up a machine that is going to be accessed and potentially also maintained by several people we should consider the best approach to this. 

This is both a question of governance (to what degree different people can have access), but also transparency and accountability (who was online when something happened). 

Note: during a working session we've noted the fine line between transparency and surveillance, however, we should probably also add the need for accountability to these concerns and discuss how we can approach and support each others mistakes when things go wrong. 

Note 2: no matter what user and access structure we choose, we should discuss how to communicate between sysadmins and how to handover work, etc. IMO in-grid is great at this. 

Luke and Mara both shared examples from their own experience and collective work with Systerserver and Varia. Both collectives made different user accounts for each person who needed access but not everyone needed sudo access. 

For Jean, we made users for everyone who needed access and we made everyone a sudo user.

Reference for how to create users and make them sudo can be found here (link to systerservers steps) **<<<<<<<<<MAYBE JUST add these steps in as well? have you got the link?**
## Names

So far we have run into two names, the username on the server and the hostname. These are Servpub01 and pubdoc respectively. 
### Changing the host name
We can change the hostname from the default rpi4 to pubdoc (during workshop 26 June) by editing the files inside /etc/hostname 

with this command:

``` shell
hostnamectl hostname <newname>
```

we chose pubdoc, so:

``` shell
hostnamectl hostname pubdoc
```

then run this to show the status and confirm the the name of the hostname:

``` shell
hostnamectl status
```


Going forward, we will also have to name:

- The physical server machine, and its other future nodes
- one public user - collaboration
- one root user (if different)
- Separate user account (named after members)
- The domain (which will be publicly accessible)

We discussed several options of naming systems: notes here: https://pad.riseup.net/p/un-Named_Server_CCI-keep


### Creating Users
##### DOCUMENTATION NEEDED HERE:
please document the steps of creating user accounts
- [ ] link to systerserver guide