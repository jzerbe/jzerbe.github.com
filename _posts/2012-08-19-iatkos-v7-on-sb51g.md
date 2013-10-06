---
layout: post
title: iATKOS v7 on SB51G
tags:
- OSx86
- iATKOS
- Shuttle
- SB51G
published: true
---
Stable Mac OS X Leopard 10.5.7 install for XCode 3 development on my
[Shuttle SB51G](http://www.shuttle.eu/_archive/older/en/sb51g.htm)
using iATKOS v7:

- [osx86.co -> iATKOS v7](http://osx86.co/f36/iatkos-v7-t3199/)
- [osx86install.com -> README](http://www.osx86install.com/support/38-iatkos-v7-readme.html)
- [kat.ph -> iATKOS v7 DVD 10.5.7 for Intel/AMD](http://kat.ph/iatkos-v7-dvd-10-5-7-for-intel-amd-t2718902.html)

Windows drivers and documentation download for good measure: <http://download.shuttle.eu/Mirror/XPC/SB51G/>


###Wake On LAN

As long as you have the _Wake On PCI Ring_ enabled in the _Power Management_ options
in your BIOS settings, Wake ON Lan magic packets sent to the machine on UDP port 9
should wake this Mac OS X install up.


###Hardware Info

- Intel 845GE+ECH4 chipset
- Socket 478 with Pentium 4 2.66GHZ cpu installed
- Realtek RTL8139 fast Ethernet NIC
- Avance AC'97 Audio
- VIA 1394
- Intel 82801DB Ultra ATA Storage Controller-24CR


###Non Working

- no audio - use for XCode development only, so does not matter
- any updates will break system - fine with XCode 3
- periodically VGA out will never display login - [connect via VNC](http://lifehacker.com/319528/remote-control-leopard-with-tightvnc) which always works


###The Config

Listed options should be checked. Triple dots mean that one or a few
checkboxes should be skipped over. Question marks mean the module is optional.

    iATKOS v7 Main System
    
    Bootloader
        Chameleon v2
    
    X86 Patches
        /Extra directory
        DSDT
        Decryptors
            AppleDecrypt
        ...
        Kernel
            9.7.0 Kernel Voodoo
        ...
        Disabler
        ...
        Remove TyMCE
    
    Drivers
        System
            SATA/IDE
                Intel SATA/IDE
            ...
            Sound
                Voodoo HDA driver <-- enabled by default, turn-off as ineffective
            PS/2 mouse/keyboard
                Apple PS/2 driver
            ...
            ext2fs?
            NTFS-3G?
        Network
            Wired
                Realtek
                    Realtek RTL8139
    
    ...
    
    Post-Install Actions
