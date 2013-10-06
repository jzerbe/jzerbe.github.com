---
layout: post
title: FreeNAS 7 CIFS lower Buffer Size and 802.11g
tags:
- FreeNAS
- CIFS
published: true
---
I was streaming mp3s from my FreeNAS 7 fileserver over CIFS the other day to a
[MusicZones](https://github.com/jzerbe/MusicZones)
zone controller and every two minutes or so there would be a break in the audio,
but not when I was pulling Internet radio directly to the
zone. Mind you this was over 802.11g, and there was no EM interference. Interestingly,
my wired zone controllers which are on standard
100Mbps Ethernet over CAT5 do not experience these audio breaks.

Turns out lowering the _Send Buffer Size_ and the _Receive Buffer Size_ from the default 64240,
to 8192 did the trick. Inspired by looking at
[old FreeNAS and Openfiler smb.conf files](http://sourceforge.net/apps/phpbb/freenas/viewtopic.php?f=46&t=17#p45).
