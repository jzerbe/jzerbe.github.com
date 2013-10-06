---
layout: post
title: AeroFS first impressions
tags:
- AeroFS
- LAN
published: true
---
Check out [AeroFS](https://www.aerofs.com/) ASAP! May require a few
business days to get an invite code, but eventually you will recieve an
automated email from an account administered by
[Yuri Sagalov (Cofounder, AeroFS)](https://www.aerofs.com/about).
It has made my LAN sync work-flow much easier. Nor are you just constrained to
the LAN:
[KB - What are the firewall requirements?](http://support.aerofs.com/knowledgebase/articles/47288-what-are-the-firewall-requirements-)


###environment

When I am home, I plug my various machines in to my 1Gbps Ethernet switch.
Up until a few weeks ago, I had an elaborate system of mouse-clicks and scripts
to curmudgen SFTP synching with my NAS and periodically push back-ups to
an external hard-drive. Painful. I only had 0.5GB of my 50GB of content
auto-magically synching with Dropbox. I really do not want to be synching 50GB
(and growing) of content across the crappy DSL line I am on.


###AeroFS NAS/headless set-up

I am running this on CentOS release 5.8 (Final). Pick up the tarball from
[https://www.aerofs.com/download?os=linux](https://www.aerofs.com/download?os=linux).

    yum install java-1.6.0-openjdk sharutils procps screen
    tar xzvf aerofs-installer.tgz
    mv aerofs /srv/AeroFS_bin
    chmod -R +x /srv/AeroFS_bin
    screen -dmS AeroFS /srv/AeroFS_bin/aerofs-cli

I added the `screen` line to `/etc/rc.local` so it
always runs on start-up. Not the best practice, but it works. You will have
to pop open the `screen` instance - `screen -r AeroFS`
- on first run to go through the CLI wizard.


###annoyances

- fight with Windows install to periodically re-remove AeroFS from start-up
- slow sync: 2-4MB/sec compared to the sustained 10MB/sec I can easily get with SFTP
- no total transfer ETA
- does not play with my 500GB NTFS volume via autofs. something about no FUSE support.
