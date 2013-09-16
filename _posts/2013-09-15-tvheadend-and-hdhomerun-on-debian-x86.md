---
layout: post
title: Tvheadend and HDHomeRun on Debian x86
tags:
- Tvheadend
- HDHomeRun
- Debian
- x86
status: draft
type: post
published: false
---
###motivation###
I have been able to build the kernel-space drivers for my HDHomeRun,
_[dvbhdhomerun](http://dvbhdhomerun.cvs.sourceforge.net/)_, yet there seem to
be some weird runtime errors having to do with cross compiling for an
ARM v7 A9 Cortex processor. I bit the bullet and rolled Tvheaden on a seperate
box. Can't watch Sunday football without a DVR.

Installing on a headless box with X11 forwarding. Doing everything as root.


###install Tvheadend##
Paraphrasing the important parts from
[AptRepository - Tvheadend](https://tvheadend.org/projects/tvheadend/wiki/AptRepository).

1. repo key: `curl http://apt.tvheadend.org/repo.gpg.key | apt-key add -`
2. append `/etc/apt/sources.list` with `deb http://apt.tvheadend.org/stable wheezy main`
3. `apt-get update; apt-get install tvheadend`


###install SiliconDust drivers###
From <http://www.silicondust.com/support/hdhomerun/downloads/linux/>

    apt-get install build-essential libgtk2.0-dev
    wget http://download.silicondust.com/hdhomerun/libhdhomerun_20130328.tgz
    wget http://download.silicondust.com/hdhomerun/hdhomerun_config_gui_20130328.tgz
    tar xzf libhdhomerun_20130328.tgz; tar xzf hdhomerun_config_gui_20130328.tgz
    cd hdhomerun_config_gui
    ./configure
    make; make install
    ldconfig


###build linux kernel###
According to [KernelFAQ](https://wiki.debian.org/KernelFAQ), Debian does
not have the standard `/proc/config.gz`, instead a plaintext file can be found
at `/boot/config-$(uname -r)`. The `make` step could take 30-90 minutes.

    apt-get install linux-source-3.2
    cd /usr/src
    tar xjvf linux-source-3.2.tar.bz2
    cd linux-source-3.2
    cp /boot/config-$(uname -r) .config
    make oldconfig; make


###install dvbhdhomerun###

    wget http://sourceforge.net/projects/dvbhdhomerun/files/dvbhdhomerun_0.0.15.tar.gz
    tar xzf dvbhdhomerun_0.0.15.tar.gz
    cd dvbhdhomerun-0.0.15/kernel

Alter the `KERNEL_DIR` variable to point to where we built the
linux kernel: `/usr/src/linux-source-3.2`.

    make
    insmod dvb_hdhomerun_core.ko
    insmod dvb_hdhomerun_fe.ko
    insmod dvb_hdhomerun.ko

`insmod` order matters.


###install userhdhomerun###

    cd ../userhdhomerun/
    apt-get install cmake

Alter the `LIBHDHOMERUN_PATH` variable to point to where you downloaded
libhdhomerun. Mine is located in `../../libhdhomerun`. `make` and then to
test it `make run`.
