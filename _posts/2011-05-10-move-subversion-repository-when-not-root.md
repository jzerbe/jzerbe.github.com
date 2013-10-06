---
layout: post
title: move subversion repository when not root
tags:
- rsvndump
published: true
---
1.  Download and install [rsvndump](http://rsvndump.sourceforge.net/)
    - install libapr-dev and libsvn-dev: `apt-get install libapr1-dev libsvn-dev`
    - the latest at time of writing: `wget http://prdownloads.sourceforge.net/rsvndump/rsvndump-0.5.5.tar.gz`
    - do the usual `./configure` after getting into the untar'd rsvndump archive (didn't work on cygwin)
    - `make`, then `make install` (as root)
2. `[rsvndump](http://rsvndump.sourceforge.net/manpage.html) -v -u [svn user] -p [svn pass] [url to svn repo] > [reponame].dump`
3. Create your new subversion repository on the new host machine and import the dump
    - `svnadmin create [reponame]`
    - `svnadmin load [reponame] < [reponame].dump`
