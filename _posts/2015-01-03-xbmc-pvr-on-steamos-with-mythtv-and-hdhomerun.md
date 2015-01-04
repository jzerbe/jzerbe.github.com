---
layout: post
title: XBMC PVR on SteamOS with MythTV and HDHomeRun
tags:
- XBMC
- PVR
- SteamOS
- MythTV
- HDHomeRun
published: true
---
One less reason to go for pay TV services: Over The Air network TV. Check the
[Digital TV Market Listings](http://www.rabbitears.info/market.php?request=station_search)
on RabbitEars.info for all the possible channels you could be getting. For more
information on what is actually showing, head over to the
[AOL TV guide listings](http://tvlistings.aol.com/listings/). For
[80237](http://tvlistings.aol.com/listings/co/denver/over-the-air/80237), they
have yet to be wrong. While I unable to get all the channels advertised with the
unpowered
[Winegard FreeVision FV-30BB HDTV Antenna](http://www.amazon.com/gp/product/B003L76BJS/)
sitting next to my coach on the floor, I am able to get the important network
affiliates - abc, CBS, Fox, NBC - and PBS in 1080i. Really only here for the PBS
and occasional sports games.


### Hardware dependencies
I configured my local DHCP server to assign the same IPv4 address to my late-model
[HDHR3-US](http://www.amazon.com/gp/product/B004HO58SO/); I did not want to find
out the hard way if [MythTV](https://www.mythtv.org/) was smart enough to track
my HDHomeRun based on GUIDs. Ran the Windows version of the
[setup software](http://www.hdhomerun.com/support/downloads/) for firmware
upgrades and available channel searching.


### Software dependencies
This assumes that you have already setup SteamOS (wrapped Debian) to pull from
standard Debian repositories. See
[SteamOS and XBMC on Intel NUC 4th gen](http://vraidsys.com/2014/04/steamos-and-xbmc-on-intel-nuc-4th-gen/)
if this is not the case. Then install a database backend:

`apt-get install [mysql-server](https://packages.debian.org/wheezy/mysql-server)`

I made the mysql instance run link-local only with root/toor for ease of config.


### Install mythtv-backend
Add the [deb-multimedia](http://www.deb-multimedia.org/) repo by adding the line
`deb http://www.deb-multimedia.org wheezy main` to `/etc/apt/sources.list`.
Be sure to add the repo key:
```bash
gpg --keyserver pgp.mit.edu --recv-keys 07DC563D1F41B907
gpg --armor --export 07DC563D1F41B907 | apt-key add -
```

Actually install the package:
```bash
apt-get update
apt-get install [mythtv-backend](http://www.deb-multimedia.org/dists/wheezy/main/binary-amd64/package/mythtv-backend)
```

Run at startup:
```bash
update-rc.d mythtv-backend defaults
```

### Configure mythtv-backend
Create a directory where you want recordings to go. I was lazy and created a
`777` directory - `/home/recordings` - on the larger `home` partition.

Then, while running a terminal session with X11 forwarding, run: `mythtv-setup`.
Be sure to run through the setup cards sequentially. Pay special attention to
[Host Address Backend Setup](http://www.mythtv.org/wiki/User_Manual:Detailed_configuration_Backend#Host_Address_Backend_Setup)
in _1. General_ and disable _Listen on Link-Local addresses_. If left on this
caused [start-up issues](https://code.mythtv.org/trac/ticket/11030). Another
sticking point: be sure to select _EIT only_ in _3. Video Sources_ for pulling
EPG data from broadcast signals.
