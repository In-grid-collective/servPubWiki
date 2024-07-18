## Install instructions on Mac:
We combined several instructions guides with the [Rosa zine guide](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf](https://psaroskalazines.gr/pdf/rosa_beta_25_jan_23.pdf) ). The reason being is it is a bit tricky on Mac and we need ***tinc version 1.1*** and macs default installers (like homebrew) will always download the latest version instead. 

### Prerequisites: 
- [Xcode](https://developer.apple.com/xcode/) (requires free online Apple Developer Membership it can also be obtained from original OSX installation CD. 
- [Macports](http://www.macports.org/install.php) for developer tools

After Macports is installed, close and reopen your terminal. Update the ports system and ports list:

``` shell
sudo port selfupdate
sudo port sync
```

To install tinc-devel using macports. Run this command:

``` shell
sudo port install tinc-devel
```

Configuration files will be located in `/opt/local/etc/tinc`

Tinc can now be configured and executed. For next steps refer to [](05-VPN%20and%20Reverse%20Proxy%20Server.md.md#Tinc%20creating%20the%20initial%20network%20and%20inviting%20nodes)


## Debug help


**_ERRORS:_**

Do you get:

error: Could not open /dev/tap0: No such file or directory 

  

Try edit the tinc.conf file to add this:

  

_DeviceType = utun_

_#No Device specified_ 

  

  

----------

Tinc Docs on conf file:

[https://www.tinc-vpn.org/documentation/tinc.conf.5](https://www.tinc-vpn.org/documentation/tinc.conf.5)

  

_DeviceType:_

  

utun (OS X)

Set type to utun. This is only supported on OS X version 10.6.8 and higher, but doesn't require the tuntaposx module. This mode should support both IPv4 and IPv6 packets.

Solution from:

[https://www.tinc-vpn.org/pipermail/tinc/2021-June/005567.html](https://www.tinc-vpn.org/pipermail/tinc/2021-June/005567.html)

  

  

Then if you manage to get it so it just says ready after trying _sudo tincd -n systerserver_ -D again you can terminate that process and use launchd below to run the service. 

  

  

On MacOS depending on the version of you operating system you might need support for tun/tap network interfaces. Newer operating systems seem to have new native support (steps provided to use that above). For older operating systems it seems the options are outdated.

  

In the past you'd need to install tuntaposx which is now an unsupported package:

[https://tuntaposx.sourceforge.net/](https://tuntaposx.sourceforge.net/) 

Further info found from here:  [](https://tunnelblick.net/cTunTapConnections.html#:~:text=all%20PowerPC%20Macs.-,What%20Apple%20announced,use%20of%20one%20system%20extension](https://tunnelblick.net/cTunTapConnections.html#:~:text=all%20PowerPC%20Macs.-,What%20Apple%20announced,use%20of%20one%20system%20extension](https://tunnelblick.net/cTunTapConnections.html#:~:text=all%20PowerPC%20Macs.-,What%20Apple%20announced,use%20of%20one%20system%20extension).

  

  

**_Automating the Starting and Stopping of our Tinc Service_**

  

This is how you can create the MacOS Equivalent of systemd to launch the tinc service (systemd is the Linux version of doing this and is what is used in the Rosa manual to do systemctl):

  

We will use launchd on MacOS:

[https://www.launchd.info/](https://www.launchd.info/)

[https://medium.com/swlh/how-to-use-launchd-to-run-services-in-macos-b972ed1e352](https://medium.com/swlh/how-to-use-launchd-to-run-services-in-macos-b972ed1e352) 

  

  

To use launchd, you have to create a property-list file for tinc and put it under /Library/LaunchDaemons/

  

The following example will be the plist file for our tinc service: **systerserver**

  

_/Library/LaunchDaemons/_systerserver_.tinc.plist:_

cd _/Library/LaunchDaemons/_

  

To create the servpub.tinc.plist in terminal, run:

    **sudo touch systerserver.tinc.plist**

We now have an empty plist ready to go!

  

Before you edit the contents of the plist and save the content read the information below the plist. Once you are happy with your settings in the plist, you can come back up here to get the commands for saving your changes using vim. 

  

- Copy the following plist content into the the terminal / vim to add the contents to the plist file we just made.

- To open the empty file to edit it with Vim on the commandline:

    **sudo vim systerserver.tinc.plist**

-Once the Vim commandline editor is open:

    press **i** to insert

- Paste the contents in. 

- To save, first press **esc (**escape**)**

- Enter **:wq**  (which stand for write and quite)

- Press enter.

- If you want to check your changes were saved do the following to print the contents of the file to your command line:

    **cat systerserver.tinc.plist**

**Plist Content:**

  

<?xml version="1.0" encoding="UTF-8"?>

<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "[http://www.apple.com/DTDs/PropertyList-1.0.dtd](http://www.apple.com/DTDs/PropertyList-1.0.dtd)">

<plist version="1.0">

<dict>

    <key>KeepAlive</key>

    <true/>

    <key>Disabled</key>

    <true/>

    <key>Label</key>

    <string>tinc.systerserver</string>

    <key>ProgramArguments</key>

    <array>

        <string>/opt/local/sbin/tincd</string>

        <string>-n</string>

        <string>systerserver</string>

        <string>-D</string>

    </array>

</dict>

</plist>

  

  

Launchd will directly start tincd if this file is found AND The <key>KeepAlive</key> element is true. KeepAlive will ensure that tincd is always running, and to be restarted if it stops. 

  

If you want to have the service start on launch remove the disabled key above. 

  

To start the service use the _launchctl_ command:

- **sudo launchctl load -w /Library/LaunchDaemons/systerserver.tinc.plist**

  

This removes the _disabled_ key from the file and starts tincd.

  

To stop tincd, use the _launchctl_ command:

- **sudo launchctl unload -w /Library/LaunchDaemons/systerserver.tinc.plist**

  

This adds a _disabled_ key to the file and stops tincd. The disabled key ensures that launchd will not start tincd anymore. 

  

  

To stop tinc:

- **launchctl stop tinc.systerserver**

  

To list all services monitored by launchd:

- **launchctl list**

  

(you might need to do **_sudo launchctl list_** to see the process running)

  

If tinc has been configured with launchd, you will see tinc.syterserver listed with a process id (this is the number on the left "column", if you see a number on the right instead that is an error code).

  

If you want a gui to monitor lahunctl using homebrew if you have homebrew already installed (tested, and very helpful in debugging!): 

- **brew install --cask launchcontrol**

  

  

F.Y.I. Homebrew is another package manager for MacOS (similar to MacPorts in a way): [https://brew.sh/](https://brew.sh/)