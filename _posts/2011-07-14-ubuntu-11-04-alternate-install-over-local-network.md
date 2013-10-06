---
layout: post
title: ubuntu (11.04) alternate install over local network
tags:
- Ubuntu
published: true
---
Digging through the Ubuntu help docs, specifically [InstallationLocalNet](https://help.ubuntu.com/community/Installation/LocalNet),
today was rather frustrating. There was not one specific (modern) way given, in said documentation: they illustrated using
[BOOTP](http://en.wikipedia.org/wiki/Bootstrap_Protocol)?!?! That said, time to make this work using (i)PXE.

The ubuntu alternate install discs do not have the [casper live system](http://www.linuxcertif.com/man/7/casper/)
which allows for the disc files to be pulled over NFS or CIFS (Samba). Although, with the existing preseed framework
(extended from Ubuntu's parent, Debian), one can pull from a local network package repository (the expanded disc image files).

I use [iPXE for various reasons](http://vraidsys.com/2011/06/network-boot-without-a-proper-dhcp-setup-for-pxe/),
instead of the generic PXE setup involving TFTP and a complicit DHCP server, which is able to use HTTP and DHCP itself
without the niceties of a LAN where you have access to the DHCP boot options.


####iPXE boot options for ubuntu studio 11.04

    kernel ubuntustudio-11.04-alternate-i386/install/netboot/ubuntu-installer/i386/linux
    initrd ubuntustudio-11.04-alternate-i386/install/netboot/ubuntu-installer/i386/initrd.gz
    imgargs linux append auto url=http://<?php echo $server_ip; ?>/tftp/boot/preseed_ubuntustudio.cfg language=en country=US console-keymaps-at/keymap=us keyboard-configuration/xkb-keymap=us keyboard-configuration/layoutcode=us console-setup/ask_detect=false --


####contents of my custom preseed_ubuntustudio.cfg

    #Debian 6 options
    d-i debian-installer/locale string en_US
    d-i console-keymaps-at/keymap select us
    d-i keyboard-configuration/xkb-keymap select us
    d-i netcfg/choose_interface select auto
    
    #Ubuntu Natty specific options
    # ubuntustudio.seed linked from ubuntustudio-11.04-alternate-i386/preseed/ubuntustudio.seed
    d-i preseed/include string ubuntustudio.seed
    
    # Installation source
    d-i mirror/country string manual
    # hostname This is whatever HTTP server you have set up.
    d-i mirror/http/hostname string bigboy
    # This is the /ubuntu directory from the install CD copied (or linked) under the webroot of your HTTP server.
    Must be "/ubuntu" otherwise this will not work.
    d-i mirror/http/directory string /ubuntu
    # Name your ubuntu version here
    d-i mirror/suite string natty
    # no proxy
    d-i mirror/http/proxy string


I am able to use the above after extracting the contents of the [ubuntustudio iso](http://ubuntustudio.org/download/)
to a location (the ubuntustudio-11.04-alternate-i386 directory) on my fileserver accessible from my HTTP server. Now I am able
to do an entirely network based boot from the contents of the ubuntu alternate disc.
