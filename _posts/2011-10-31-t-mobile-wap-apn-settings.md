---
layout: post
title: T-Mobile WAP APN settings
tags:
- Motorola
- T-Mobile
published: true
---
**NOTE**: [no more free T-Mobile web service with keyword hole](https://twitter.com/jasonzerbe/status/182675503294070784).

I have been playing around with my new Android 2.2.1 dev phone, trying to port over the free webaccess I am
able to get on my old Motorola J2ME feature-phones (Razr V3r, V195s).

Interesting that these two devices have slightly different APN settings. Able to mess around with these settings
after using P2KCommander v6 to modify the seem to show the _Web Sessions_ menu and allow setting the default session.

On my V195s:

    Name: t-zones
    Homepage: http://wap.myvoicestream.com
    Service Type 1: HTTP
    Proxy 1: 216.155.165.050
    Port 1: 8080
    Domain 1: [empty]
    Service Type 2: WAP
    Proxy 2: 216.155.165.050
    Port 2: 9201
    Domain 2: [empty]
    DNS 1: 000.000.000.000
    DNS 2: 000.000.000.000
    Timeout: 5 minutes
    CSD No. 1: [empty]
    User Name 1: [empty]
    Password 1: [empty]
    Speed (Bps) 1: 9600
    Line Type 1: ISDN
    CSD No. 1: [empty]
    User Name 2: [empty]
    Password 2: [empty]
    Speed (Bps) 2: 9600
    Line Type 2: ISDN
    GPRS APN: wap.voicestream.com
    User Name: motV190
    Password: motV190

On my Razr V3r everything else is the same except for the username and password:

    User Name: motV3
    Password: motV3

And from the
[automatic MCC and MNC detection app](http://vraidsys.com/2011/11/automatic-mcc-and-mnc-detection-using-motorola-j2me-enabled-cellphone-gsm/)
I made:

    MCC = 310, MNC = 260

I am able to enter all these values into the APN settings on my smartphone, but last time I tried to connect the
settings were erased. I suppose I will need to do some setprop shenanigans to re-enter the settings once the
T-Mobile tower erases them.
