---
layout: post
title: recover sd card data with linux and gddrescue
tags:
- gddrescue
- data
- recovery
published: true
---
Just to be clear about the setup, I\'ll be using Debian 5 Linux and an 8 GB SD card with a SD to USB adapter over a USB 2.0 link.

gddrescue is included in the main [Debian](http://packages.debian.org/search?keywords=gddrescue) and
[Ubuntu](http://packages.ubuntu.com/search?keywords=gddrescue) repos, so in my case the install was just
`apt-get install gddrescue`.

Plug in your sd card and watch dmesg for the sd card. Mine was added to /dev/sda as one can see from the following output:

    [ 2540.580007] usb 5-4: new high speed USB device using ehci_hcd and address 2
    [ 2540.776619] usb 5-4: configuration #1 chosen from 1 choice
    [ 2540.781484] scsi1 : SCSI emulation for USB Mass Storage devices
    [ 2540.781939] usb 5-4: New USB device found, idVendor=090c, idProduct=6000
    [ 2540.781947] usb 5-4: New USB device strings: Mfr=1, Product=2, SerialNumber=3
    [ 2540.781953] usb 5-4: Product: USB2.0 Card Reader
    [ 2540.781957] usb 5-4: Manufacturer: Generic,   .
    [ 2540.781961] usb 5-4: SerialNumber: 12345678901234567890
    [ 2540.781977] usb-storage: device found at 2
    [ 2540.781980] usb-storage: waiting for device to settle before scanning
    [ 2545.780245] usb-storage: device scan complete
    [ 2545.781307] scsi 1:0:0:0: Direct-Access Generic 6000 PQ: 0 ANSI: 0 CCS
    [ 2545.783697] sd 1:0:0:0: [sda] 15855616 512-byte hardware sectors (8118 MB)
    [ 2545.784973] sd 1:0:0:0: [sda] Write Protect is off
    [ 2545.784983] sd 1:0:0:0: [sda] Mode Sense: 4b 00 00 08
    [ 2545.784988] sd 1:0:0:0: [sda] Assuming drive cache: write through
    [ 2545.787318] sd 1:0:0:0: [sda] 15855616 512-byte hardware sectors (8118 MB)
    [ 2545.787949] sd 1:0:0:0: [sda] Write Protect is off
    [ 2545.787958] sd 1:0:0:0: [sda] Mode Sense: 4b 00 00 08
    [ 2545.787963] sd 1:0:0:0: [sda] Assuming drive cache: write through
    [ 2545.788050] sda: unknown partition table
    [ 2545.795255] sd 1:0:0:0: [sda] Attached SCSI removable disk</code>

While logged in as root (or using sudo) make a copy of the sd card: ddrescue [/dev/device] [recovery image output] [logfile]

Now we will also need testdisk, so install that [Debian](http://packages.debian.org/search?keywords=testdisk) or
[Ubuntu](http://packages.ubuntu.com/search?keywords=testdisk): `apt-get install testdisk`

[Testdisk](http://www.cgsecurity.org/wiki/TestDisk) can do some pretty cool stuff, but we are more
interested in one of the subpackages it includes, [photorec](http://www.cgsecurity.org/wiki/PhotoRec).

Now using photorec, let\'s recover some photos: `photorec /d [photo output dir] [recovery image output file from before]`
And navigate through the menus and get your photos back. Usually the sd card is formatted in NTFS.
