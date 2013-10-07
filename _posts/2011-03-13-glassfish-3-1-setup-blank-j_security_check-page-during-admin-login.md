---
layout: post
title: glassfish 3.1 setup and blank j_security_check page during admin login
tags:
- Debian
- J2EE
- GlassFish
status: publish
type: post
published: true
meta:
_edit_last: "1"
---
After reading up on the basics of the [JSTL](http://www.ibm.com/developerworks/java/library/j-jstl0211.html)
and various other JSP stuffs, I thought it was time I run my own glassfish development server. Do note that the paths
and commands may not work exactly for your distro, this is only a log of the problem points that I hit upon while getting
[GlassFish Server Open Source Edition - 3.1 Final](http://glassfish.java.net/downloads/3.1-final.html) running on
Debian Squeeze (6.0).

Doing this on Debian, I was met with the usual Java + Linux hate. Had to configure apt to use the contrib and non-free packages,
just to be sure to get the most up-to-date binaries and the official Sun/Oracle JRE and JDK (`apt-get install sun-java6-jdk`).

You also need to modify `/etc/profile` to include the java6 binaries in your path:

    ...
    if [ "`id -u`" -eq 0 ]; then
    PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
    else
    PATH="/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games"
    fi
    PATH=$PATH:/usr/lib/jvm/java-6-sun/bin
    export PATH
    ...

Now Debian should be cool and let you install glassfish with just a `sh glassfish-3.1-unix.sh`.
Be sure to have __X11 forwarding on__ if you are running this on a remote VM like I was, as this
is the __graphical installer__.

Once everything was installed and running I noticed that I was using the admin interface over http, well that won\'t
do, so I went to enable Security (using the admin domain interface). glassfish crapped its pants, giving me a blank
page "j_security_check" and the log files said that a user was trying to log in with blank credentials. It turns out
that you are supposed to use the shell script `glassfish/bin/asadmin` in your glassfish3 install directory
to connect to your glassfish3 domain server instance -
`bin/asadmin --user [admin|your admin username] --passwordfiledomains/[your main domain]/config/admin-keyfile --secure=true`
- and execute the command `enable-secure-admin`. See the glassfish project JIRA ticket
[changing admin-listener security-enabled attribute breaks REST, GUI and cannot stop domain](http://java.net/jira/browse/GLASSFISH-16142?page=com.atlassian.jira.plugin.system.issuetabpanels%3Achangehistory-tabpanel).
