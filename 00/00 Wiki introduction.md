
These docs make up the process and steps that made up the back-end of our technical configuration. They act not only as a resource as to how to take up and hack these things yourself, but also to present alongside this the histories, networks and inspirations that have led us to this infrastructure's existence.

### Access

We have tried to make these docs accessible and legible (as possible) to both technical and non-technical readings. If you want a light, but still deep exploration of the setup, check out the [Index of Pages](https://wiki4print.servpub.net/index.php?title=Docs:Contents#Index_of_Pages) listed below.

### Prerequisites

If you would like to follow along, the following are necessary:

- **Small board computer (SBC)
	- We used a raspberry pi, although You don't necessarily need to use a pi to create a collaborative environment, you could use another type of computer. To understand more about why we chose to use a pi, you can find our notes here`[Chapter 2a: Server Issues: Platform Infrastructure](https://wiki4print.servpub.net/index.php?title=Chapter_2a:_Server_Issues:_Platform_Infrastructure)`
* **Peripherals**
	* HDMI screen, keyboard, mouse etc. 
* **A laptop or other personal computer**
	* It would be good to have SSH installed on your laptop. Most OS have it by default now, if not then manually install following the steps under [[01.3 SSH]]
* **Knowledge of terminal/bash** 
	* For an intro to basic terminal/bash commands see [[05 Appendix/NA-Terminal Unix Commands Cheat Sheet|NA-Terminal Unix Commands Cheat Sheet]] 

### Note on terms

Throughout these docs we'll be referring to the server and the client. The client-server model in this context refers to a structure where tasks/workloads are distributed between the provider of a resource of service, and clients, who want to access that resource.

When we come to talk about [[01.3 SSH|SSH]]:

- Server = Raspberry Pi/SBC (Single board computer)
	The server which holds the files/resources etc you want to access
- Client = your laptop, 
	- which you are using to access those files/resources

Later, we talk about [[02 Section Index|Local server setup]]. In this case this is a client-server protocol where:

* Server = Raspberry Pi/SBC
	* An http-server which holds files/resources you want to access
* Client = A device with access to the same local network as the Pi
	* which will access those files/resources via a web browser

This is confusing. We discussed how we might unpack this terminology, in a way which is critical but also legible and appropriate for the function we have assigned these docs. We came to the conclusion we will keep it simple here, but we do discuss this in more detail in the book chapter Praxis Doubling if you would like to read more on this.


### Tuxic & domains

The first step that we took in this process was the purchase of the servpub.net domain name from [tuxic.nl](https://tuxic.nl/), we have therefore placed some introductory information about setting up domain names here.



- [ ] Why we have put this here Winnie first step, can we include some writing that Winnie is doing about Tuxic for platform infrastructure? Radical reference? started drafting, see below
- [ ] Summary of the step for setting up tuxic.. Do we have any info about setting up the A record and pointing it to Jean's static IP. So maybe we just mention that we pointed the domain A record to the static IP. 
- [ ] Add link to Network terminology
- [ ] 