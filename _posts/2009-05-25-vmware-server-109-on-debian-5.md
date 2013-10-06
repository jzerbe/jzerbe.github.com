---
layout: post
title: vmware server 1.09 on Debian 5
tags:
- Debian
- VMware
published: true
---
###copy-paste###

    apt-get install linux-headers-`uname -r` libx11-6 libx11-dev x-window-system-core x-window-system xspecs libxtst6 psmisc build-essential xinetd gcc-4.1 g++-4.1
    ln -sf /usr/bin/gcc-4.1 /usr/bin/gcc
    wget http://download3.vmware.com/software/vmserver/VMware-server-1.0.9-156507.tar.gz
    tar xzvf VMware-server-1.0.9-156507.tar.gz
    cd vmware-server-distrib/
    ./vmware-install.pl

Follow the instructions; the defaults should be just fine.

    cd ..

Download [vmware-update-2.6.26-5.5.7.tgz.gz](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvdlVNZWNkUlVkX0E)

    tar -xzvf vmware-update-2.6.26-5.5.7.tgz.gz
    cd vmware-update-2.6.26-5.5.7
    ./runme.pl

Follow the instructions; the defaults should be just fine.

    cd ..
    rm -fdr /tmp/*
    rm -fdr vmware*
    rm -fdr VMware*
    shutdown -r now


###explanation###
1. All the dependencies were installed first
2. System link was created so that the old version of gcc is used that is compatible with the kernel headers
3. VMare server software is downloaded and installed
4. Reach a point in the installation where the process errors out installing the vmmon module
5. Use the patcher to override some dependency stuffs that can't be fixed
6. Once the process is done, remove the installation files, and then restart the system to free up the memory used by the installation procedure
