---
layout: post
title: Netbeans 7.0 Python support
tags:
- Netbeans
- Python
published: true
---
Upgrading to Netbeans 7.0 had a bunch of perks, my personal favorites being: the faster cold and warm start-up times,
Git support, and (what seems like) faster C++ library refresh/load times. The down-side to this update was
the lack of out-of-the-box Python support. Fortunately there is a quick fix for this.

From Wade Chandler, in response to [_Howto activate python in NB 7.0_](http://forums.netbeans.org/ptopic38275.html)
on the netbeans forums:
> You can use the dev auto update center. Python seems to still work though I'm a python newbie:
> <http://deadlock.netbeans.org/hudson/job/nbms-and-javadoc/lastStableBuild/artifact/nbbuild/nbms/updates.xml.gz>

So, from the main NetBeans IDE window menu bar:

1.  Tools->Plugins (which opens the Plugins window)
2.  Select the "Settings" tab
3.  Hit the "Add" button (bottom right)
4.  enter "Last Stable Build" and <http://deadlock.netbeans.org/hudson/job/nbms-and-javadoc/lastStableBuild/artifact/nbbuild/nbms/updates.xml.gz>
5.  go back to the "Updates" tab and hit the "Reload Catalog" button
6.  This should give you access to the latest stable build (development) plugins, which should allow you to install the Python plugins of your choosing
