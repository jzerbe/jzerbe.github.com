---
layout: post
title: Counter Strike Debian linux server setup
tags:
- Counter Strike
- Debian
published: true
---
### Counter Strike server setup procedure ###
Based upon - <http://www.cstrike-planet.com/tutorial/1/5> - Props to whoever wrote it!

All you have to do is setup the Counter Strike dedicated server to
[run on different port/IP addresses](http://forums.steampowered.com/forums/showthread.php?t=292495&amp;page=9#post_message_8842952).

1. Create an install directory and dl the installer
    - mkdir srcds (the directory for Counter Strike: Source dedicated server)
    - cd srcds
    - wget http://www.cstrike-planet.com/dls/hldsupdatetool.bin
2. Make the installer executable and execute it
    - chmod +x hldsupdatetool.bin
    - ./hldsupdatetool.bin
3. Agree to the terms and conditions by typing _yes_ and _steam_ will be created in the same directory
4. Now run the steam installer that will dowload a new version of HLDSUpdateTool
    - `chmod +x steam`
    - `./steam` - it took about 10 minutes for it to update. All depends upon the steam server loads.
5. We are now ready to download and install Counter Strike Source. The install size is ~ 1.1GB uncompressed.
    - `./steam -command update -game _Counter-Strike Source_ -dir .`
6. The server is now ready for various customizations.


### Counter Strike 1.6 linux dedicated server ###
Based upon - <http://www.cstrike-planet.com/tutorial/1-Linux-Install-CS-16/6>

1. Repeat steps 1-5 in previous installation but in different root directory. so instead of _srcds_ let\'s put the
installation in _hlds_. Step 2 of the previous method should this time be:
    - `mkdir hlds`
    - `cd hlds`
2. Run the various steps outlined above, and now to install the (~360MB) cstrike1.6 linux dedicated server.
    - `./steam -command update -game cstrike -dir`


### Q and A time ###

1. How can I keep hlds running while I am not logged into the server with SSH?
    - Start your dedicated server with screen: `screen -S hlds ./hlds_run`. And then when you want to access the running server process once logged back in: `screen -r hlds`.
2. How many CS virtual servers can I get on one machine?
    - <http://www.webhostingtalk.com/showthread.php?t=743585>
3. What does the _tickrate_ have anything to do with anything? What is it?
    - <http://www.counter-strike.com/tickrate.php>
