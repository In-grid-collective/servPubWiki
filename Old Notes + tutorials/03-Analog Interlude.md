# Analog Interlude
_you do not need to be at your machine for this bit_

So now we know how to connect to a server machine that is on the same network as our personal machine. The next "logical" step would be to figure out how to do this _across_ networks but we won't do that yet. 

We have to take an analog interlude to decide on two key things that will make the future steps a lot easier.  Even if we don't make the final decision about this now, now is a good time to consider them and we can come back to them later.
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
# Creating Users

Using [this guide](https://alexandria.anarchaserver.org/index.php/Access_server) from Systerserver, we can create users and give them different access levels. 
Before creating any new users, make sure you are at root level (or the root user) of the machine. To do that, do:
```shell
sudo su
```

To make a new user, use the command below. Note, that you will be prompted to input a password and it is always better to give different users passwords
```shell
adduser <nameofuser>
```

If you want to give this user sudo access, then they have to be added to the "sudo" group. You don't need to create this group, it exists by default and you can just add or remove users from it. The sudo group is stored in this directory: /etc/sudoers.d/
```bash
usermod -aG sudo <nameofuser>
```

You can check to see who is in the group sudo using this command:
```bash
 grep '^sudo:.*$' /etc/group
```

Now you can change into the new user (in other words, sign into their account):
```shell
su <nameofuser>
```