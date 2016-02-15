---
layout: post
title: VMware Server 1.0.10 on CentOS 5.8
tags:
- VMware
- CentOS
published: true
---
I am running old x86 hardware and the disk drivers for CentOS 6 just do not jive
with my setup. I would use Debian, but the install process for VMware Server
is janky on non-RHEL. I want to run Windows XP and various Linux distros with as
little [hypervisor](http://en.wikipedia.org/wiki/Hypervisor) overhead
as possible. This means I have been choosing VMware Server 1.0.10
[EOL](http://www.centos.org/modules/newbb/viewtopic.php?topic_id=33920&forum=56#forumpost145817)
for the past few years. Unlike the version 2 RAM-hog, the last release of version 1 continues
to work great for my purposes. VMware no longer links to the download page from
their search results or site nav, but fortunately I have kept it bookmarked:
[Download VMware Server (for Windows and Linux systems)](http://register.vmware.com/content/download-1010.html).


### setup on a minimal CentOS 5.8 install

The following should install the necessary build tools and development file
dependencies needed to create the various VMware Server binaries. 25MB via yum.

    yum install make gcc kernel-devel xinetd
    yum groupinstall 'X Software Development'


### the install pastie

Download (102MB), extract, install, configure. The install Perl script should
automatically redirect you to the installed vmware-config.pl; defaults should be
fine for the install and config scripts.

Linux version 1.0.10 serial number: 98RD1-Y6HD9-2FH65-4KLUH

    wget http://download3.vmware.com/software/vmserver/VMware-server-1.0.10-203137.tar.gz
    tar xzvf VMware-server-1.0.10-203137.tar.gz
    cd vmware-server-distrib/
    ./vmware-install.pl
