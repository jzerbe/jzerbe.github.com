---
layout: post
title: Android 2.2.1 (custom) rooting and DNS not resolving
tags:
- Android 2.2.1
- root
published: true
---
I finally caved and bought an Android smartphone for $87 (including shipping) last week.
Was it a good purchase? Perhaps. I suppose it was inevitable, as I work on webapps targeting both iOS
and Android 2.X+ devices. I bought a
[Qualcomm Snapdragon Mobile Development Platform](http://www.cnx-software.com/2011/01/21/qualcomm-snapdragon-mobile-development-platform-available/)
from a bloke off of eBay; probably cost him more than $87 (closer to $500+).

Unfortunately this device had not been rooted, nor were there any DNS servers set when I connected to my home 802.11g
home network. ALSO: Android Marketplace is not installed, so had to grab all the .apks from non-standard channels.
Not sure if the previous owner screwed this up or what?

What needed to get done:

1. get root access to phone to set DNS servers
2. automate process of setting DNS servers when connecting to Wi-Fi networks
3. download some apps and updates

###rooting the phone

Since I was able to access direct IPv4 addresses, what I did was upload my various .apk files to my LAN
webserver (http://192.168.x.x/path) and download them to my device. Bluetooth-to-Bluetooth connection would not let my
transfer .apk\'s. Android 2.2.1 is allegedly just for patching up rooted devices, and allegedly one has to downgrade
to 2.2 to root. But I had success by 1) installing [Universal Androot](http://blog.23corner.com/tag/universalandroot/)
1.6.2 beta 5 ([local mirror](https://docs.google.com/folder/d/0B0yT30uCaFvvcVhpaHFmcy1BMWs/edit?pli=1#docId=0B0yT30uCaFvvR2g4WHB6UEJUWVU))
([screen shots and more info](http://blog.varunkumar.me/2010/08/one-click-root-for-nexus-one.html))
and attempting to root and it failing (I removed the Universal Androot after and kept the bundled SuperUser app which is handy)
and then 2) installing z4root ([local mirror](https://docs.google.com/folder/d/0B0yT30uCaFvvcVhpaHFmcy1BMWs/edit?pli=1#docId=0B0yT30uCaFvvZDRyc3dwMlozVzQ))
([screen shots and more info](http://forum.xda-developers.com/showthread.php?t=833953))
and successfully rooting in that order.


###automating DNS resolver (background)

Admittedly this solution is rather hackish, but since I do not have access to the Android Marketplace I cannot
get the slick DNS settings apps like [mytechie\'s SetDNS utility](https://market.android.com/details?id=uk.co.mytechie.setDNS&hl=en).
Also, if you do not have access to a terminal emulator of some kind, this solution might not be for you. If you want something more
stable or want more information see the article on [Varun\'s ScratchPad called _Change the DNS server of 3G connection on Android phones_](http://blog.varunkumar.me/2010/09/how-to-change-dns-server-of-3g.html).
My pay-as-you go T-Mobile service has no problem setting my device\'s DNS servers when I access their
[Edge network](http://en.wikipedia.org/wiki/Enhanced_Data_Rates_for_GSM_Evolution).


###automating DNS resolver (setup)

Open up the Terminal Emulator\'s startup commands, you might already see something like `export $PATH ...`. Put a semi-colon at the end
of the line if there is not already one and append the following:
> `su -c "setprop net.dns1 208.67.222.222"; su -c "setprop net.dns2 208.67.220.220";`
So whenever you open up your terminal emulator application, it should automatically set your DNS resolvers to
[OpenDNS](http://www.opendns.com/)\'s servers. On my own device I put in the first resolver address as one of my
university\'s DNS resolvers, as they have internal addresses for authenticating devices when you connect to their 802.11b/g/n mesh.


###some other apps

1. Flash 10.1 - The .apk should work on any Android 2.2
phone - [local mirror](https://docs.google.com/file/d/0B0yT30uCaFvvZUhaQVdJNjJJUEU/edit?pli=1),
[source information](http://www.droid-life.com/2010/08/23/download-official-flash-10-1-v10-1-92-8-for-froyo-now/)
2. rootexplorer_2.13.3.apk -
[local mirror](https://docs.google.com/file/d/0B0yT30uCaFvvMzhuYkxQV0FzNFU/edit?pli=1) - great root file browsing app
