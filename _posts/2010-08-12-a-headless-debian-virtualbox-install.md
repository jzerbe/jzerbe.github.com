---
layout: post
title: a headless Debian Lenny VirtualBox install
tags:
- Debian
- VirtualBox
published: true
---
After extreme disappointment with VMware Server 2, I thought I might try setting up a headless VirtualBox install
with a hopefully faster web interface. I need to be able to run Windows Server 2008 instances on occasion to
troubleshoot and test my scripts on IIS7. I also am looking for a more regularly updated virtualization software
to take over my aging VMware server 1.10 installs.

Much of the following is based off of
[VBoxHeadless - Running Virtual Machines With VirtualBox 3.1.x On A Headless Debian Lenny Server](http://www.howtoforge.com/vboxheadless-running-virtual-machines-with-virtualbox-3.1.x-on-a-headless-debian-lenny-server),
[instructions Debian Backports](http://www.backports.org/dokuwiki/doku.php?id=instructions),
and the [VirtualBox.org website's information about Debian VBox setups](http://www.virtualbox.org/wiki/Linux_Downloads).
FYI: this install was done on a fresh Lenny netinstall (logged in as root ... duh!), and the apt-get install command grabed
about 200MB worth of compressed packages that unarchive into about 500MB worth of disk space used.

    wget -q http://download.virtualbox.org/virtualbox/debian/oracle_vbox.asc -O- | apt-key add -
    wget -q http://backports.org/debian/archive.key -O- | apt-key add -
    echo -e "ndeb http://download.virtualbox.org/virtualbox/debian lenny non-freendeb http://www.backports.org/debian lenny-backports main contrib non-free" &gt;&gt; /etc/apt/sources.list
    apt-get update
    apt-get install virtualbox-3.2 dkms
    groupadd vboxers
    useradd -d /home/vboxers -m -g vboxers -s /bin/bash vboxers
    passwd vboxers
    adduser vboxers vboxusers
    apt-get install libapache2-mod-php5
    a2dissite default
    mkdir -p /srv/vbox
    chmod -R 777 /srv
    echo -e "<VirtualHost *:80>nDocumentRoot /srv/vboxn</VirtualHost>" > /etc/apache2/sites-enabled/vbox
    /etc/init.d/apache2 restart
    apt-get install screen

Open up the local start up file `/etc/rc.local` and insert
`su vboxers -c "screen -dmS vboxwebsrv vboxwebsrv` before the `exit 0`.
Download the phpvirtualbox code <http://phpvirtualbox.googlecode.com/files/phpvirtualbox-0.5.zip>,
unzip and modify the config.php file to match the vboxers user we set up earlier. Finally upload the contents
of the archive to the vbox directory over SFTP or something. Reboot the machine (for good measure),
and you should be able to go to http://host/ to access the web interface. Beware this a completely unprotected setup!
