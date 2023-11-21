
This section is here to clarify some of the terminology we use throughout the subsequent section.

### IP addresses

IP (Internet Protocol) addresses work like a bit like a street address in that they allow devices find each other online. Every internet device has a unique IP address. These addresses can be converted into text via a Domain Name System (DNS) format. ``See`` [[04-Static IP & Domain Setup#Domains]] ``for more info on Domains.``

There are are two types of IP addresses - **static** and **dynamic** - which each have their own use cases. The main difference is in their names. Dynamic IP addresses constantly change, and static IP addresses are fixed. 

#### Dynamic IP addresses

Dynamic IPs are the standard for most consumer devices, or those connected to your home network like your phone or laptops. Devices are assigned IPs in the background by your internet service provider. These IPs are assigned temporarily, and change without you noticing. 

#### Static IP addresses

Static IP addresses are unchanging - an IP is assigned to a device and it remains the same. They are manually assigned to individual devices, and are in place for as long as you need them to be. They usually also come with a cost associated. They are used for things like DNS servers, which need to remain accessible and devices always know where to find them. As they are fixed, static IP addresses also make it easier to work with Virtual Private Networks (VPNs). In our case Systerserver provide the static IP we use for servepub. 
#### Domain names

If IP addresses are the addresses that devices use to find each other online, then domains are how we understand where things are online. When you access a server on the internet you usually do so using its domain name. Domain names are easier and more identifiable for us to remember, and recognise than IP addresses. servpub.net is easier for us to remember for example than a string of numbers. A DNS server translates between domain names (which we understand) and IP addresses (which our devices understand). 

** did we buy the domain name?


#### HTTP versus HTTPS

Hypertext Transfer Protocol (HTTP) is a method for encoding and transporting information between a client (for instance a web browser) and a web server. HTTPS does the same thing but with encryption and verification using digital certificates. HTTP messages are plaintext, which means unauthorised parties could access and read them. HTTPS messages are encrypted so that if they are intercepted they cannot be understood. 

HTTPS websites must obtain an SSL/TS certificate from a certificate authority (CA)

#### DNS Records

DNS records provide information about a hostname or domain, and include the IP address for a domain. They are stored on a DNS server.

There are several types of DNS record. 5 main types include:

- A record
- AAAA record
- CNAME record
- Nameserver (NS) record
- Mail exchange (MX) record

The A record is the most key. The A stands for address and the shows the IP address for a specific hostname or domain. An A records main use is to allow a web browser to load a website from their domain name, without us having to know its IP address. 

An AAAA record is similar, only the DNS record points to an IPV6 address.

CNAME records points a domain name to another domain. For instance, a CNAME record can point the address www.example.com_ to the actual web site for the domain example.com.

An NS record specifies the authoritative DNS server for a domain.

MX records show where emails for a domain should be routed to i.e. directing emails to a mail server.

-----------------------------------------

Keep reading for creating a network of nodes using a VPN. 

