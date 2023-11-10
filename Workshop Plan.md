### For Workshop:
**check on internet access for pi in the room**
#### Intro (30mins)
- Introduction to servPub and its players.
- Round the table intros
- Overview and Diagrams of servpub
- This project welcomes feedback on accessibility and learning methods
	- ACTION: Shared pad. 
##### Network Anatomy (30 mins)
- Introduction to local network + Introduce network anatomy briefly (guided by diagrams, reference "Local Config" in servPub documentation).

Tour of the browser:
- ACTION SLIDE: Join Dongle Wi-fi (don't do big data things... joke about mbs)
-  Type url with IP of an html file on the pi
	- URL BAR: explain IP
	- URL BAR: explain directories and files
	- what we see in the browser -> diagram as ASCII image hosted on the server (pre-made)
##### To-do:
- [ ] make visual material describing project & network relations
- [ ] path diagram of what we will learn
#### Sharing Feminist Methodologies (30 mins)
Sharing feminist methodologies (Systerserver)

ACTION: Reminder if you are on Windows download Git for Windows so you can follow along on terminal. We can help you during the break. 
#### BREAK (15 mins)
#### Working Session:

##### Intro to the Terminal  (20mins)
- We should introduce Terminal 
- Explain what Linux is (if you are on windows install Git Bash) -> why is commonly used for environments
-  using some commands to ssh into local pi
- key Terminal commands needed 
	- sudo 
	- cd -> change to the directory of the ascii art page
	- ls -> list out the files -> index.html
	- cat -> print file content to terminal
	- nano file -> edit a file with nano 
- Intro tmux -> join tmux session
##### Tinc: mount the 2nd Pi (50 mins)
- We have installed Tinc -> we did aptget & installed [[03-Web Server Setup#Tinc]] for linux
- Configuring the pi for Tinc [[03-Web Server Setup#Configuration]]
- Watch us on Jean add the 2nd Pi
- Someone pings Jean from the 2nd Pi using the local VPN IP
##### To-do:
- [ ] Prep 2nd pi: flashing SSD card + Install OS
- [ ] Prep 2nd pi: maybe create one public user for participants & give it a fun password
- [ ] Decide what parts of tech setup to do in workshop & make steps
	- Local SSHing and local webpage - with attendee participation
- [ ] make wiki into slides 
#### BREAK (15 mins)

#### Documentation & Discussion (30 mins)
- Review of shared pad

To-do:
- [ ] Documentation of ServPub pending completion:
	- [ ] Becky: NginX and automation
	- [ ] Batool: Creating users here: [[02-Analog Interlude]]
	- [ ] certification [[04-Domain basic setup]]
	- [ ] missing steps of getting certificates [[04-Domain basic setup]]
	- [ ] redirecting http to https [[04-Domain basic setup]]