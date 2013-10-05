---
layout: post
title: vmware server 1.09 on Debian 5
tags:
- Debian
- VMware
published: true
---
Prerequisites: clean Debian 5 installation (preferably from netinstall disc), reliable Internet connection<br />
<br />
Copy/Paste Code block:
<blockquote>
    <code>apt-get install linux-headers-`uname -r` libx11-6 libx11-dev x-window-system-core x-window-system
        xspecs libxtst6 psmisc build-essential xinetd gcc-4.1 g++-4.1<br />
        ln -sf /usr/bin/gcc-4.1 /usr/bin/gcc<br />
        wget http://download3.vmware.com/software/vmserver/VMware-server-1.0.9-156507.tar.gz<br />
        tar xzvf VMware-server-1.0.9-156507.tar.gz<br />
        cd vmware-server-distrib/<br />
        ./vmware-install.pl<br />
        [ !-- follow the instructions. the defaults should be just fine. --! ]<br />
        cd ..<br />
        [ !-- download <a href="https://docs.google.com/file/d/0B0yT30uCaFvvdlVNZWNkUlVkX0E/edit?pli=1">vmware-update-2.6.26-5.5.7.tgz.gz</a> --! ]<br />
        tar -xzvf vmware-update-2.6.26-5.5.7.tgz.gz<br />
        cd vmware-update-2.6.26-5.5.7<br />
        ./runme.pl<br />
        [ !-- follow the instructions. the defaults should be just fine. --! ]<br />
        cd ..<br />
        rm -fdr /tmp/*<br />
        rm -fdr vmware*<br />
        rm -fdr VMware*<br />
        shutdown -r now
    </code><br />
    <br />
    <i>Edited July 30, 2009</i></blockquote>
Here is what just happened:<br />
1) all the dependencies were installed first<br />
2) then a system link was created so that the old version of gcc is used that is compatible with the kernel headers<br />
3) the vmware server software is downloaded and installed.<br />
4) you should reach a point in the installation where the process errors out installing the vmmon module.<br />
5) use the patcher to override some dependency stuffs that can't be fixed.<br />
6) once the process is done, remove the installation files, and then restart the system to free up the memory used by the installation procedure.<br />
