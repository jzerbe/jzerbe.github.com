---
layout: post
title: recover sd card data with linux and gddrescue
tags:
- gddrescue
- data
- recovery
published: true
---
Just to be clear about the setup, I'll be using Debian 5 Linux and an 8 GB SD card with a SD to USB adapter over a USB 2.0 link.<br />
<br />
gddrescue is included in the main <a href="http://packages.debian.org/search?keywords=gddrescue">Debian</a> and
<a href="http://packages.ubuntu.com/search?keywords=gddrescue">Ubuntu</a> repos, so in my case the install was just
an "<code>apt-get install gddrescue</code>".<br />
<br />
Plug in your sd card and watch dmesg for the sd card. Mine was added to /dev/sda as one can see from the following output:
<blockquote><code>[ 2540.580007] usb 5-4: new high speed USB device using ehci_hcd and address 2<br />
    [ 2540.776619] usb 5-4: configuration #1 chosen from 1 choice<br />
    [ 2540.781484] scsi1 : SCSI emulation for USB Mass Storage devices<br />
    [ 2540.781939] usb 5-4: New USB device found, idVendor=090c, idProduct=6000<br />
    [ 2540.781947] usb 5-4: New USB device strings: Mfr=1, Product=2, SerialNumber=3<br />
    [ 2540.781953] usb 5-4: Product: USB2.0 Card Reader<br />
    [ 2540.781957] usb 5-4: Manufacturer: Generic,   .<br />
    [ 2540.781961] usb 5-4: SerialNumber: 12345678901234567890<br />
    [ 2540.781977] usb-storage: device found at 2<br />
    [ 2540.781980] usb-storage: waiting for device to settle before scanning<br />
    [ 2545.780245] usb-storage: device scan complete<br />
    [ 2545.781307] scsi 1:0:0:0: Direct-Access Generic 6000 PQ: 0 ANSI: 0 CCS<br />
    [ 2545.783697] sd 1:0:0:0: [sda] 15855616 512-byte hardware sectors (8118 MB)<br />
    [ 2545.784973] sd 1:0:0:0: [sda] Write Protect is off<br />
    [ 2545.784983] sd 1:0:0:0: [sda] Mode Sense: 4b 00 00 08<br />
    [ 2545.784988] sd 1:0:0:0: [sda] Assuming drive cache: write through<br />
    [ 2545.787318] sd 1:0:0:0: [sda] 15855616 512-byte hardware sectors (8118 MB)<br />
    [ 2545.787949] sd 1:0:0:0: [sda] Write Protect is off<br />
    [ 2545.787958] sd 1:0:0:0: [sda] Mode Sense: 4b 00 00 08<br />
    [ 2545.787963] sd 1:0:0:0: [sda] Assuming drive cache: write through<br />
    [ 2545.788050] sda: unknown partition table<br />
    [ 2545.795255] sd 1:0:0:0: [sda] Attached SCSI removable disk</code></blockquote>
While logged in as root (or using sudo) make a copy of the sd card: ddrescue [/dev/device] [recovery image output] [logfile]<br />
<br />
Now we will also need testdisk, so install that [<a href="http://packages.debian.org/search?keywords=testdisk">Debian</a>]
[<a href="http://packages.ubuntu.com/search?keywords=testdisk">Ubuntu</a>]: <code>apt-get install testdisk</code><br />
<br />
<a href="http://www.cgsecurity.org/wiki/TestDisk">Testdisk</a> can do some pretty cool stuff, but we are more
interested in one of the subpackages it includes, <a href="http://www.cgsecurity.org/wiki/PhotoRec">photorec</a>.<br />
<br />
Now using photorec, let's recover some photos: <code>photorec /d [photo output dir] [recovery image output file from before]</code>
And navigate through the menus and get your photos back. Usually the sd card is formatted in NTFS.
