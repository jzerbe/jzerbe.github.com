---
layout: post
title: network boot without a proper DHCP setup for PXE
tags:
- iPXE
published: true
---
I needed an alternative to the slickness of a network-wide PXE setup. Router DHCPd doesn\'t allow for modifications.
If you can stand to throw in a bootable usb or floppy, then the following might work for you.

1) Make your existing tftp directory accessible over HTTP and said HTTP server should be able to run php scripts.
I currently have nginx and lighttpd\'s spawn-fcgi being overseen by supervisor, very similar to
[Setting up PHP5/FastCGI with nginx](http://agiletesting.blogspot.com/2010/06/setting-up-php5fastcgi-with-nginx.html).

2) Throw up a php script in the same directory where your `pxelinux.0` is located. Check out my example
[ipxe_boot.php](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvWjM5cWtaUFZyU0k).
Check the GDoc comments for using TFTP if you experience freeze-up problems when booting a VM like I did or as
described in [VMware/VirtualBox is freezing when loading a kernel/initrd](http://forum.ipxe.org/showthread.php?tid=5514).

3) Create a custom .ipxe boot script for adding into your iPXE boot image. For more on
[scripting with iPXE](http://ipxe.org/scripting); also take a look at my current
[custom.ipxe script](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvZkJMYWFCaG5wSjA).

4) Grab a copy of the source code from the [download/build info page](http://ipxe.org/download)
and build it with your custom .ipxe file attached.

    git clone git://git.ipxe.org/ipxe.git
    cd ipxe/src

Bootable floopy disk:

    make bin/ipxe.dsk EMBEDDED_IMAGE=../../custom.ipxe
    cat bin/ipxe.dsk > /dev/fd0

Bootable usb PXE replacement:

    make bin/ipxe.usb EMBEDDED_IMAGE=../../custom.ipxe
    dd if=bin/ipxe.usb of=/dev/sdb

Do be sure that your usb drive is actually /dev/sdb!
