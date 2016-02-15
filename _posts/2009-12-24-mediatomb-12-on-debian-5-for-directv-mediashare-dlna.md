---
layout: post
title: MediaTomb 12 on Debian 5 for DirecTV MediaShare/DLNA
tags:
- DirecTV
- DLNA
- MediaTomb
published: true
---
### copy-paste ###
    apt-get update
    apt-get upgrade
    apt-get -y install screen vlc vorbis-tools mpg123 ffmpeg unzip subversion build-essential
    autoconf automake curl libexpat-ocaml-dev libsqlite3-dev libcurl-ocaml-dev libid3-3.8.3-dev
    libtagc0-dev libavformat-dev libexif-dev
    svn co https://svn.mediatomb.cc/svnroot/mediatomb/trunk/mediatomb mediatomb
    cd mediatomb
    autoreconf -i
    ./configure
    make
    make install
    cd

Download [mediatomb.zip](0B0yT30uCaFvvaEhaU0JQd2tsa28).

    mv mediatomb.zip /etc/
    cd /etc/
    unzip mediatomb.zip
    rm mediatomb.zip
    mkdir /var/lib/mediatomb
    chmod -R 777 /var/lib/mediatomb/
    cd
    mv /etc/mediatomb/mediatomb /etc/init.d/

Change line in `/etc/init.d/mediatomb` so ethX reads eth0, eth1, or whatever your LAN network interface card is.

    chmod +x /etc/init.d/mediatomb
    update-rc.d mediatomb defaults

### explanation ###
The idea is that the default MediaTomb install found in the repos (version 11) doesn\'t support transcoding
quite right for the DirecTV HR2x. So we do an install from the repos for a bunch of dependencies for the
latest code (version 12) that we then checkout from the project SVN repo. A new configuration script
that I built is downloaded and installed. It has support for transcoding ogg, mp3, flac, flv, avi, mkv, mov,
and a few other formats (pretty easy to add support for other formats check out
<http://mediatomb.cc/dokuwiki/transcoding:transcoding#directv_hr2x_transcoding>).

MediaTomb will be automatically started on boot. It can also be stopped and started manually by:
`/etc/init.d/mediatomb stop` or `/etc/init.d/mediatomb start`.

You can then go to the webui of MediaTomb and update your media library: <http://hostname:49152/>
