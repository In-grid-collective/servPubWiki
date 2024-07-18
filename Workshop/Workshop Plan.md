### For Workshop:
**check on internet access for pi in the room**
#### Intro (30mins)
- Introduction to servPub and its players. -batool
- Round the table intros -batool
- Overview and Diagrams of servpub -batool
- This project welcomes feedback on accessibility and learning methods -batool - george
	- ACTION: Shared pad. 

#### Sharing Feminist Methodologies (20 mins)
Sharing feminist methodologies (Systerserver)

##### Network Anatomy (30 mins) - becky + kt
- Introduction to local network + Introduce network anatomy briefly (guided by diagrams, reference "Pi for Local Network Access" in servPub documentation).

Tour of the browser:
- ACTION SLIDE: Join Dongle Wi-fi (don't do big data things... joke about mbs)
-  Type url with IP of an html file on the pi
	- URL BAR: explain IP
	- URL BAR: explain directories and files
	- what we see in the browser -> diagram as ASCII image hosted on the server (pre-made)
##### To-do:
- [ ] make visual material describing project & network relations

ACTION: Reminder if you are on Windows download Git for Windows so you can follow along on terminal. We can help you during the break. 
#### BREAK (15 mins)
#### Working Session:

##### Intro to the Terminal  (20mins) - becky
- We should introduce Terminal 
- Explain what Linux is (if you are on windows install Git Bash) -> why is commonly used for environments
-  using some commands to ssh into local pi
- key Terminal commands needed 
	- sudo 
	- cd -> change to the directory of the ascii art page
	- ls -> list out the files -> index.html
	- cat -> print file content to terminal
	- nano file -> edit a file with nano 
	- ssh -> pw login (if install is needed then can also introduce apt/mac: homebrew or other example)
- Intro tmux -> join tmux session
##### Tinc: mount the 2nd Pi (50 mins) - George - b200l + kt = chaos
- We have installed Tinc -> we did aptget & installed [](02-Web%20Server%20Setup%20on%20Pi.md#Tinc) for linux
- Configuring the pi for Tinc [](02-Web%20Server%20Setup%20on%20Pi.md#Configuration)
- Watch us on Jean add the 2nd Pi
- Someone pings Jean from the 2nd Pi using the local VPN IP
##### To-do:
- [x] Prep 2nd pi: flashing SSD card + Install OS
- [ ] Prep 2nd pi: maybe create one public user for participants & give it a fun password
- [x] Decide what parts of tech setup to do in workshop & make steps
	- Local SSHing and local webpage - with attendee participation
- [x] make wiki into slides 
#### BREAK (15 mins)

#### Documentation & Discussion (40 mins) - B200l + George 

- Review of shared pad

- at bottom of new pad add comments linking to pages#headings. 

- give people 10 - 15 minutes to write some feedback:
	- Allow people to choose a section and give people  free space to give feedback in pairs.
	- Proposed talking points:
		- accessibility
		- legibility
		- conceptual sitting/critique
		- histories, situatedness
		- network materiality - resources/dependencies
		- collective working
		- potential uses, futures
	- create prompts of different aspects of the docs we can critique (eg. accesiblities, relevance to histories of servers, etc.)
	- Which parts needs more info (visuals or text)
	- Input on how to make collaborative documents / document collaborative work.

- Can provide big paper for people to make drawing

- Discuss how-tos of making documentation

To-do:
- [ ] Documentation of ServPub pending completion:
	- [ ] Becky: NginX (ongoing) and automation
	- [ ] Batool: Creating users here: [03-Analog Interlude](03-Analog%20Interlude.md)
	- [ ] certification [04-Static IP & Domain Setup](04-Static%20IP%20&%20Domain%20Setup.md)
	- [ ] missing steps of getting certificates [04-Static IP & Domain Setup](04-Static%20IP%20&%20Domain%20Setup.md)
	- [ ] redirecting http to https [04-Static IP & Domain Setup](04-Static%20IP%20&%20Domain%20Setup.md)
	- [ ] Add proper / full credits for Rosa Manual.
	- [ ] Draft a "this is what you need to do before the workshop if you want to follow along with the terminal activity"
	- [ ] Proof read / add to the Terminal commands cheat sheet, nano user in particular please [NA-Terminal Unix Commands Cheat Sheet](NA-Terminal%20Unix%20Commands%20Cheat%20Sheet.md)