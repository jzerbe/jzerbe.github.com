---
layout: post
title: classic gaming on emulated Mac OS 7 in Windows
tags:
- Mac OS 7
- 1990s
- classic gaming
published: true
---
Up until two weeks ago, it had been 13 years since last I laid eyes on Mac OS 7
and all the awesome games that I remembered playing at the age of 8.
My grandparents had an Apple
[PowerBook 520c](http://en.wikipedia.org/wiki/PowerBook_500_series)
that they would bring along on trips, and a
[Macintosh Performa 6400](http://en.wikipedia.org/wiki/Macintosh_Performa)
that inhabited their basement. My grandpa was going through old stuff I guess and brought out
the very same PowerBook this last weekend. Maybe I should have grabbed it as a collector\'s item,
but I already have quite the computer graveyard; I decided to see if I couldn\'t
play these games in an emulated manner instead.

According to [Macintosh Garden\'s overview guide](http://macintoshgarden.org/guides),
one needs Basilisk II.
> Basilisk II emulates a 680x0 Macintosh that will run MacOS versions 7 to 8.1.
> This represents a date range of about 1991 to 1996.
> Games published in the early and mid 1990s are most stable in Basilisk II.

For the most part I followed the linked guide on emaculation.com for
getting Basilisk II running on my Windows XP machines:
[_Setting up Basilisk II for Windows_](http://www.emaculation.com/doku.php/basilisk_ii_setup).

1. From the [_BasiliskII for Windows build 30-08-2010 available_](http://www.emaculation.com/forum/viewtopic.php?t=5282)
        forum post one is directed to download and extract
        [BasiliskII_15_01_2010.zip](http://www.open.ou.nl/hsp/downloads/BasiliskII_15_01_2010.zip)
2. To configure Basalisk II via the GUI, download and install
        [gtk+-2.10.13-setup.exe](http://www.emaculation.com/basilisk/gtk+-2.10.13-setup.exe)
3. Follow along with the aforementioned guide until you reach the _JIT Compiler_,
        be sure to uncheck _Translate through constant jumps_, as it makes the
        emulated CPU calculate incorrect hashes for the System 7.5.3 installation.

Feel free to download my Windows emulated Mac OS 7.5.5 setup
[BasiliskII.zip](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvS01aZFFJYVh4eE0)
which includes
[Stuffit Expander 5.5](http://www.emaculation.com/basilisk/stuffit_expander_55.bin)
[Lemmings](http://mac.thebasingers.com/software/Lemmings.sit),
[Spectre Challenger](http://www.macheaven.net/forum/viewtopic.php?f=4&t=703),
and [Spin Doctor](http://www.macheaven.net/forum/viewtopic.php?f=4&t=711).
Upon unzipping the archive, you will just need to correct the __Mac OS 7.hfv__ and
__Mac OS ROM__ locations via the Basalisk II Gui.
