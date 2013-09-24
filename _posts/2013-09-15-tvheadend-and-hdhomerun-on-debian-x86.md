---
layout: post
title: Tvheadend and HDHomeRun on Debian x86
tags:
- Tvheadend
- HDHomeRun
- Debian
- x86
status: publish
type: post
published: true
---
###motivation###
I have been able to build the kernel-space drivers for my HDHomeRun,
_[dvbhdhomerun](http://dvbhdhomerun.cvs.sourceforge.net/)_, yet there seem to
be some weird runtime errors having to do with cross compiling for an
ARM v7 A9 Cortex processor. I bit the bullet and rolled Tvheaden on a seperate
box. Can't watch Monday night football without a DVR.

Installing on a headless box with X11 forwarding. Doing everything as root.
Partially inspired from
[Building – dvbhdhomerun](http://sourceforge.net/apps/trac/dvbhdhomerun/wiki/Building).



###install dev dependencies###

    apt-get install cvs build-essential cmake



###install Tvheadend##
Paraphrasing the important parts from
[AptRepository - Tvheadend](https://tvheadend.org/projects/tvheadend/wiki/AptRepository).

1. repo key: `curl http://apt.tvheadend.org/repo.gpg.key | apt-key add -`
2. append `/etc/apt/sources.list` with `deb http://apt.tvheadend.org/stable wheezy main`
3. `apt-get update; apt-get install tvheadend`



###install SiliconDust drivers###
Get the latest version from
<http://www.silicondust.com/support/hdhomerun/downloads/linux/>

    apt-get install build-essential libgtk2.0-dev
    wget http://download.silicondust.com/hdhomerun/libhdhomerun_20130328.tgz
    wget http://download.silicondust.com/hdhomerun/hdhomerun_config_gui_20130328.tgz
    tar xzf libhdhomerun_20130328.tgz; tar xzf hdhomerun_config_gui_20130328.tgz
    cd hdhomerun_config_gui
    ./configure
    make; make install
    ldconfig

OR the easier version (might be slightly out of date)

    apt-get install libhdhomerun-dev hdhomerun-config



###setup linux kernel source###
According to [KernelFAQ](https://wiki.debian.org/KernelFAQ), Debian does
not have the standard `/proc/config.gz`, instead a plaintext file can be found
at `/boot/config-$(uname -r)`.

    apt-get install linux-source-3.2
    cd /usr/src
    tar xjvf linux-source-3.2.tar.bz2
    cd linux-source-3.2
    cp /boot/config-$(uname -r) .config
    make oldconfig
    make prepare scripts



###install dvbhdhomerun###

    wget http://sourceforge.net/projects/dvbhdhomerun/files/dvbhdhomerun_0.0.15.tar.gz
    tar xzf dvbhdhomerun_0.0.15.tar.gz
    cd dvbhdhomerun-0.0.15/kernel

Alter the `KERNEL_DIR` variable in the _Makefile_ to point to where
we setup the linux kernel: `/usr/src/linux-source-3.2`.

    make
    make install
    modprobe dvb_hdhomerun

From <http://dvbhdhomerun.cvs.sourceforge.net/viewvc/dvbhdhomerun/dvbhdhomerun/build.txt?view=markup>

You will also want to add `dvb_hdhomerun` to load at startup.
Edit the `/etc/modules` file and create a new line with
just `dvb_hdhomerun` on it.



###install userhdhomerun###

    cd ../userhdhomerun/

If you manually built _libhdhomerun_, alter the _Makefile_
`LIBHDHOMERUN_PATH` variable to point to where you downloaded
libhdhomerun. Mine is located in `../../libhdhomerun`.

    make; make install

