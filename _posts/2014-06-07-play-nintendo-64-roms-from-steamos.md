---
layout: post
title: play Nintendo 64 ROMs from SteamOS
tags:
- SteamOS
- Debian
- Nintendo 64
published: true
---
Best way to launch emulators from dual SteamOS/XBMC HTTPC is definately from
the Steam client side. Learned the hard-way that XBMC attaches to audio
interfaces and does not want to let go. This will update Steam client app tiles
with shortcuts to launch various ROMs in the appropriate emulator.

This assumes that you have already setup SteamOS (wrapped Debian) to pull from
standard Debian repositories. See
[SteamOS and XBMC on Intel NUC 4th gen](http://jzerbe.com/2014/04/steamos-and-xbmc-on-intel-nuc-4th-gen/)
if this is not the case.

### Get
- `apt-get install git mupen64plus python`
- `git clone --depth=1 https://github.com/jzerbe/Ice.git`
- `git checkout steamos`

### Configure
- Update [`Ice/config.txt`](https://github.com/jzerbe/Ice/pull/1/files#diff-0)
to match the root directory of where your ROMs are stored.
- Ensure
[directory structure](http://scottrice.github.io/Ice/getting-started/#adding-roms)
matches nicknames of console. With the
[stock configuration](https://github.com/jzerbe/Ice/pull/1/files#diff-f454b79607a545fa5766ffa8dd45ebedR62),
you are going to want a `N64` directory.
- Be that sure `.n64` and `.z64` filenames match against hits in
[Steam Banners](http://steambanners.booru.org/).

### Run
- Exit to the desktop and kill - `kill -s KILL #PID#` - the steam process.
Might respawn the Steam client, in which case run the `kill` command again,
and then should exit to the desktop automatically.
- In the cloned `Ice` directory, execute `python ice.py` to update
`/home/steam/.local/share/Steam/userdata/########/config/shortcuts.vdf`.
May have to delete empty parallel directories; other `userdata/########`
that have an empty `config` directory.

