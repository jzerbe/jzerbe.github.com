---
layout: post
title: automatic MCC and MNC detection using Motorola J2ME enabled cellphone [GSM]
tags:
- Motorola
- J2ME
published: true
---
Been playing around with the [APN](http://en.wikipedia.org/wiki/Access_Point_Name)
settings on my Qualcomm Android dev phone. I wanted to make sure I had the right _Mobile Country Code_
and _Mobile Network Code_, since I knew I had the correct network id.

I threw together a J2ME app,
[MotorolaCellInfo.jar](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvd1BNNFAxTGpBUVE),
that detects these values from my SIM card when I use it in my Motorola phone. This uses functionality that is
only guaranteed to work on Motorola devices. For more, please see the source I used:
_[Getiing Location Information (IMEI,MCC,MNC,LAC,CELLID) without GPS](http://ajay-mobileappdev.blogspot.com/2008/05/getiing-network-information-without-gps.html)_.

If you are the hacker type, check out the NetBeans project+source
[zip](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvOTZ3UHNqeXhsY2s).
