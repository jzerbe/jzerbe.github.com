---
layout: post
title: rtorrent 11 on Debian 5
tags:
- rtorrent
- Debian
published: true
---
For those of you looking for DHT support with the default rtorrent package that it is in the Debian
5 apt repos you are out of luck. Currently the only way to get DHT support for rtorrent is to use the
0.8.5-2 release for Debian Squeeze (testing).

So to get rtorrent working with DHT, go about the installation like you would usually:
    apt-get update
    apt-get upgrade
    apt-get -y install rtorrent
    apt-get remove rtorrent libtorrent10

The remove should leave dependencies installed.

    wget http://http.us.debian.org/debian/pool/main/r/rtorrent/rtorrent_0.8.6-1_i386.deb
    wget http://http.us.debian.org/debian/pool/main/libt/libtorrent/libtorrent11_0.12.6-2_i386.deb
    wget http://http.us.debian.org/debian/pool/main/o/openssl/libssl0.9.8_0.9.8o-4squeeze1_i386.deb
    dpkg -i libtorrent11_*.deb
    dpkg -i rtorrent_*.deb
    dpkg -i libssl*.deb

Your version of rtorrent, should now have DHT and PEX support.

For automatically starting rtorrent, you can download an init script from the libtorrent project.

    wget http://libtorrent.rakshasa.no/raw-attachment/wiki/RTorrentCommonTasks/rtorrentInit.sh
    mv rtorrentInit.sh /etc/init.d/rtorrent
    chmod +x /etc/init.d/rtorrent

You will need to change the user in the config file to one that has a `.rtorrent.rc` in it\'s home directory.
Then stick in the line `/etc/init.d/rtorrent start` before the `exit 0` in `/etc/rc.local`.

Make a .rtorrent.rc startup file, so that /etc/init.d/rtorrent works properly.
Download [rtorrent.rc](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvb1dYWmNpQWNZNGc).

    mv rtorrent.rc /home/[user]/.rtorrent.rc

Change the user you want rtorrent to run as in the `.rtorrent.rc` file.

    mkdir -p /home/[user]/torrents/session
    chown [user] /home/[user]/.rtorrent.rc
    chmod -R 777 /home/[user]/torrents

Now restart the machine: `shutdown -r now`

When you log back in, torrents should be automatically downloaded to the 
`/home/[user]/torrents/` directory, when you stick .torrent files in the
`/home/[user]/torrents/` directory. You can also access the screen thread by
`screen -r rtorrent` in a terminal session.
