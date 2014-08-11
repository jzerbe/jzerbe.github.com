---
layout: post
title: SteamOS and XBMC on Intel NUC 4th gen
tags:
- SteamOS
- XBMC
- Intel NUC
- AeroFS
published: true
---
Putting SteamOS and XBMC on an Intel NUC last weekend was surprisingly
straightforward for what is billed as __very much__ a DIY project.
I just needed to pick up a PS/2 to USB converter for an old PS/2 keyboard.

###The Hardware
- [Intel NUC D54250WYK1 Intel 4th Gen Core i5-4250U Processor with Power Cord](http://www.amazon.com/gp/product/B00H3YT8CC)
- [Crucial 16GB Kit DDR3/DDR3L 1600 MHz](http://www.amazon.com/gp/product/B008LTBJFW)
max RAM the NUC will take is 16GB (2x8GB)
- [Crucial M500 240GB mSATA Internal SSD](http://www.amazon.com/gp/product/B00BQ8RKT4)

Pretty hard to screw up the assembly portion; just glance at the instructions.

###BIOS Update
The NUC needs a newer BIOS to properly handle the UEFI Debian boot binary.
I opted to follow the
[F7 BIOS update instructions](http://www.intel.com/support/motherboards/desktop/sb/CS-034522.htm),
which entails hitting __F7__ at BIOS load time and hitting a file on your
USB drive. My unit was on __2013.1017__, and updating to __2014.0303__
worked. Do note, that one will have to __disable the Wake-On-Lan__ feature, until
Intel releases a fix for the [reboot after shutdown issue](https://communities.intel.com/thread/47751).

###SteamOS installation
1. Download [SteamOSInstaller.zip](http://repo.steampowered.com/download/SteamOSInstaller.zip).
As noted on [Build your own Steam Machine](http://store.steampowered.com/steamos/buildyourown).
2. Unzip the contents to the root of your FAT32-formatted USB drive.
3. Boot from the UEFI entry on the USB drive into the installer.
4. Run the _Automated Install_ to overwrite the contents of the SSD.

###Add Debian Wheezy repos and XBMC
This is based upon the Steam Universe posts:
[Installing applications from the Debian repo in SteamOS](http://steamcommunity.com/groups/steamuniverse/discussions/1/648814396114274132/),
and [Intergrating XBMC in SteamOS](http://steamcommunity.com/groups/steamuniverse/discussions/1/648816742742587380/).

1. Get into a sudo shell: `sudo -s`
2. Open up `/etc/apt/sources.list` and add:
    - `deb http://ftp.us.debian.org/debian/ wheezy main contrib non-free`
    - `deb-src http://ftp.us.debian.org/debian/ wheezy main contrib non-free`
    - `deb http://ftp.us.debian.org/debian/ wheezy-backports main contrib non-free`
3. Update apt: `apt-get update`
4. Install XBMC: `apt-get -t wheezy-backports install xbmc`
5. Update the gnome session files to automatically switch between Steam and XBMC on either program exit:
    - `mv /usr/share/xsessions/gnome.desktop /usr/share/xsessions/gnome-old.desktop`
    - `cp /usr/share/xsessions/XBMC.desktop /usr/share/xsessions/gnome.desktop`
6. Restart

###Install AeroFS for Music syncing
While I filled up an external NTFS USB drive with Videos, I like to keep my
music library synced across all my devices with AeroFS.
__Updated__ August 10 2014.

1. Install the Java 7 runtime: `apt-get install openjdk-7-jre-headless`
2. Grab the `.deb` from [https://www.aerofs.com/downloading?os=linux](https://www.aerofs.com/downloading?os=linux)
3. Install it: `dpkg -i aerofs-installer-*.deb`
4. Install screen: `apt-get install screen`
5. Create the file `~/launch_aerofs.sh` with the contents:

        #!/bin/bash
        export LANG=en_US.UTF-8
        screen -dmS AeroFS aerofs-cli1

6. Create the file `~/.xbmc/userdata/autoexec.py` with the contents:

        import os
        os.system("/home/desktop/launch_aerofs.sh")

7. AeroFS will always be running in the background after 1st open of XBMC.
More on [XBMC wiki: Autoexec.py](http://wiki.xbmc.org/index.php?title=Autoexec.py).

###Additional customizations
- Install the NTFS-3g drivers for read-write NTFS drive support: `apt-get install ntfs-3g`
- Add [Xbox 360 Controller keymapping](http://forum.xbmc.org/showthread.php?tid=135871) to XBMC.
Keymap XML mirrored on my GDrive:
[beta.2.0.19.keymap.xbox.360.controller.xml](https://drive.google.com/file/d/0B0yT30uCaFvvWWJXOGU3WjZPZGs/edit?usp=sharing).
