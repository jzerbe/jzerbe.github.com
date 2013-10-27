---
layout: post
title: setup Tvheadend with DirecTV electronic program guide
tags:
- Tvheadend
- XMLTV
- electronic program guide
- EPG
published: true
---
I could not seem to find any documentation on the
[electronic program guide](https://tvheadend.org/projects/tvheadend/wiki/Electronic_Program_Guide)
scraper configuration for the North America DTV XMLTV scraper. It needed to be documented.


### CLI part

1. Log in and `tv_grab_na_dtv --configure`. This should bring up the DTV EPG grabber configuration wizard.
2. Fill in the information it asks, and be ready to spend awhile looking through the channels. I spent the time going
    through each channel [the DTV guide](http://www.directv.com/guide) provides for _80237_ to decrease the fetch time;
    ultimately I only wanted EPG information for the local off-air channels. If you are in the _80237_ zip, my
    [tv_grab_na_dtv.conf](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvMG9hTjFBVDNrRjQ)
    should have all the off-air channels.
3. If you are logged in to another user besides the `hts` user that Tvheadend creates during the package install,
    then `cp ~/.xmltv/tv_grab_na_dtv.conf /home/hts/.xmltv/tv_grab_na_dtv.conf`. Do be sure to
    `chown hts /home/hts/.xmltv/tv_grab_na_dtv.conf`.
4. Start Tvheadend.


###Tvheadend part

![Tvheadend DirecTV EPG grabber config](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvVUt5ZmF2S3VZVXc)

I increase the _Grab interval_ when I first start Tvheadend to populate EPG data quicker. Usually, I leave the interval
in the range of about 3 hours. One could have it even longer, as 1-2 weeks of EPG data come in.
