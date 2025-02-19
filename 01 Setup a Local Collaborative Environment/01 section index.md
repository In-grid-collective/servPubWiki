
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





- [x] Should the users and collective work be here?
- [x] Make appendix section, make intro section
- [x] General notes, more guiding language
- [x] Terminal bash in the appendix
- [ ] Find link to the rosa server manual
- [ ] Can we link to our platform infra workshop here?