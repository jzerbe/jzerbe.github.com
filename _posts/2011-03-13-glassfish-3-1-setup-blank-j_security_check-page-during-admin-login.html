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
After reading up on the basics of the <a href="http://www.ibm.com/developerworks/java/library/j-jstl0211.html">JSTL</a>
and various other JSP stuffs, I thought it was time I run my own glassfish development server. Do note that the paths
and commands may not work exactly for your distro, this is only a log of the problem points that I hit upon while getting
<a href="http://glassfish.java.net/downloads/3.1-final.html">GlassFish Server Open Source Edition - 3.1 Final</a> running on Debian Squeeze (6.0).<br />
<br />
Doing this on Debian, I was met with the usual Java + Linux hate. Had to configure apt to use the contrib and non-free packages,
just to be sure to get the most up-to-date binaries and the official Sun/Oracle JRE and JDK (<code>apt-get install sun-java6-jdk</code>).
You also need to modify <code>/etc/profile</code> to include the java6 binaries in your path:
<blockquote>...
    if [ "`id -u`" -eq 0 ]; then<br />
    PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"<br />
    else<br />
    PATH="/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games"<br />
    fi<br />
    PATH=$PATH:/usr/lib/jvm/java-6-sun/bin<br />
    export PATH<br />
    ...
</blockquote>
Now Debian should be cool and let you install glassfish with just a <code>sh glassfish-3.1-unix.sh</code>.
Be sure to have <strong>X11 forwarding on</strong> if you are running this on a remote VM like I was, as this
is the <strong>graphical installer</strong>.<br />
<br />
Once everything was installed and running I noticed that I was using the admin interface over http, well that won't
do, so I went to enable Security (using the admin domain interface). glassfish crapped its pants, giving me a blank
page "j_security_check" and the log files said that a user was trying to log in with blank credentials. It turns out
that you are supposed to use the shell script <code>glassfish/bin/asadmin</code> in your glassfish3 install directory
to connect to your glassfish3 domain server instance -
<code>bin/asadmin --user [admin|your admin username] --passwordfiledomains/[your main domain]/config/admin-keyfile --secure=true</code>
- and execute the command "<code>enable-secure-admin</code>". See the glassfish project JIRA ticket
"<a href="http://java.net/jira/browse/GLASSFISH-16142?page=com.atlassian.jira.plugin.system.issuetabpanels%3Achangehistory-tabpanel">changing admin-listener security-enabled attribute breaks REST, GUI and cannot stop domain</a>".
