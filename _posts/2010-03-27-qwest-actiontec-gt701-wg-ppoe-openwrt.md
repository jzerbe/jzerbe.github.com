---
layout: post
title: qwest Actiontec gt701-wg PPOE on OpenWRT
tags:
- Actiontec
- OpenWRT
published: true
---
I was having a difficult time yesterday trying to get my new [OpenWRT](http://openwrt.org/) install
working on my Qwest DSL line. Here is how I got things working. I have yet to get the wireless interface
working with WPA, I\'ve just been using a different wireless access point I had lying around.

Telnet into your gateway before flashing it and `cat /proc/[###]/cmdline` (where ### is the process number of pppd),
this will get you the exact command used, along with the user/password that your gateway is using to
login to the DSLAM. Save this command string for later!

I used the latest
[Kamikaze release _as of 03/28/2010_](http://downloads.openwrt.org/kamikaze/8.09.2/ar7/openwrt-ar7-squashfs.bin)
and my Windows XP laptop connected via an Ethernet switch to my Actiontec. I was having trouble with tnftp on Debian,
but I\'ve heard it isn\'t too hard to get that working. You have to be pretty fast, because as soon as you plug in
your gateway you have ~3 seconds to ftp in. The username/password is adam2/adam2. No matter what IPv4 you have your
gateway normally set to, it defaults to 192.168.0.1 on boot.

    C:\Documents and Settings\Administrator\Desktop>ftp 192.168.0.1
    Connected to 192.168.0.1.
    220 ADAM2 FTP Server ready.
    User (192.168.0.1:(none)): adam2
    331 Password required for adam2.
    Password:
    230 User adam2 successfully logged in.
    ftp> binary
    200 Type set to I.
    ftp> quote SETENV mtd5,0x90010000,0x903e0000
    200 SETENV command successful
    ftp> quote SETENV MAC_PORT,0
    200 SETENV command successful
    ftp> quote MEDIA FLSH
    200 Media set to FLSH.
    ftp> put "openwrt-ar7-squashfs.bin" "openwrt-ar7-squashfs.bin mtd5"
    200 Port command successful.
    150 Opening BINARY mode data connection for file transfer.
    226 Transfer complete.
    ftp: 2621444 bytes sent in 33.95Seconds 77.21Kbytes/sec.
    ftp> quote REBOOT
    221-Thank you for using the FTP service on ADAM2.
    221 Goodbye.
    Connection closed by remote host.
    ftp> quit

Wait about 20 seconds before you `telnet 192.168.1.1` to set the initial root password.
Once you set the root password you are only allowed to login via SSH or the HTTP web interface.

SSH in to your new install and remove the existing options in `/etc/ppp/options` on your new OpenWRT install,
and put the options we extracted earlier in the `/etc/ppp/options` file.

My `/etc/ppp/options` file ended up looking like this:

    user [username]@qwest.net
    password [password]
    nodetach
    defaultroute
    usepeerdns
    mru 1492
    maxfail 10
    lcp-echo-failure 4
    lcp-echo-interval 30

And my `/etc/config/network` file for the wan portion:

    config atm-bridge
    option unit     0
    option encaps   llc
    option vpi      0
    option vci      32
    option payload  bridged # some ISPs need this set to 'routed'
    
    
    config interface wan
    ##      PPPoE:
    option ifname   nas0
    option proto    pppoe
    
    ##      PPPoA:
    #       option ifname   atm0
    #       option proto    pppoa
    option encaps   vc
    option vpi      0
    option vci      32

That should wrap things up. You might have to customize a few other things.
