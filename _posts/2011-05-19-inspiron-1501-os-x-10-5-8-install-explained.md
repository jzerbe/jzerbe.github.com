---
layout: post
title: Inspiron 1501 OS X 10.5.8 install explained
tags:
- Inspiron 1501
- OSX
published: true
---
###Background

A month ago I was able to successfully install
a working OSX 10.5.8 install on my Dell Inspiron 1501 for a more enjoyable development experience. Having a
bunch of scripting languages, sdks, gnu tools, all wrapped in a sexy UI makes for a lovely time. I finally
took the time to document said process. Be forewarned however that dual-booting with a Linux distribution
or Windows could make the network interfaces have issues.

I essentially followed this guide -
[Installation of OS X 10.5.8](http://sites.google.com/site/osx1501/inspiron-1501-guide/mac-os-x-leopard) -
to get a working system, but there are a few things left out. All the files that are mentioned (which you will need) in the
above tutorial I made local copies which you can also obtain here:
[osx-inspiron-1501_drivers.zip](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvbnRFeTJRcFkzWDg).


###Dual Booting

I found the easiest way to get dual booting working properly with OSX and Windows XP was to 1) format the HDD
with gparted (can be found as an [independent bootable system](http://gparted.sourceforge.net/livecd.php)
or a componenet of many Linux distros) - create a HFS+ and NTFS partition, 2) install Windows XP, and then 3) install
OSX from your iDeneb disc (so that the Chameleon bootloader is installed properly).


###Installing the Audio Driver with the Kext Helper

What the tutorial fails to state is that: you should _not start_ the Kext Helper application by _double clicking_,
but should instead _drag the codec text dump_ (Sigmatel9200.text) _onto the application icon_, which will then
start the application and patch the system. See
[Linux codec dump for SigmaTel 9200 (How to do?)](http://www.insanelymac.com/forum/lofiversion/index.php/t44972.html)
on the insanelymac.com forum for more information on creating your own codec dump
(probably not necessary as I found out the hard way).


###Quartz missing

The Mac OSX [Quartz (graphics layer)](http://en.wikipedia.org/wiki/Quartz_(graphics_layer)) does not
work with the Inspiron 1501 hardware. Thus, several noteable visual applications do not work properly, namely VLC.
Instead of installing VLC, I was able to play many of the same files with [Perian](http://www.perian.org/)
installed (in conjunction with QuickTime).
