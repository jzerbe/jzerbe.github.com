---
layout: post
title: T-Mobile WAP APN settings
tags:
- Motorola
- T-Mobile
published: true
---
<strong>NOTE</strong>: <a href="https://twitter.com/jasonzerbe/status/182675503294070784">
no more free T-Mobile web service with keyword hole</a>.<br />
<br />
I have been playing around with my new Android 2.2.1 dev phone, trying to port over the free webaccess I am
able to get on my old Motorola J2ME feature-phones (Razr V3r, V195s).<br />
<br />
Interesting that these two devices have slightly different APN settings. Able to mess around with these settings
after using P2KCommander v6 to modify the seem to show the "Web Sessions" menu and allow setting the default session.<br />
<br />
On my V195s:
<blockquote><code>Name: t-zones<br />
        Homepage: http://wap.myvoicestream.com<br />
        Service Type 1: HTTP<br />
        Proxy 1: 216.155.165.050<br />
        Port 1: 8080<br />
        Domain 1: [empty]<br />
        Service Type 2: WAP<br />
        Proxy 2: 216.155.165.050<br />
        Port 2: 9201<br />
        Domain 2: [empty]<br />
        DNS 1: 000.000.000.000<br />
        DNS 2: 000.000.000.000<br />
        Timeout: 5 minutes<br />
        CSD No. 1: [empty]<br />
        User Name 1: [empty]<br />
        Password 1: [empty]<br />
        Speed (Bps) 1: 9600<br />
        Line Type 1: ISDN<br />
        CSD No. 1: [empty]<br />
        User Name 2: [empty]<br />
        Password 2: [empty]<br />
        Speed (Bps) 2: 9600<br />
        Line Type 2: ISDN<br />
        GPRS APN: wap.voicestream.com<br />
        User Name: motV190<br />
        Password: motV190</code></blockquote>
On my Razr V3r everything else is the same except for the username and password:
<blockquote><code>User Name: motV3<br />
        Password: motV3</code></blockquote>
And from the <a href="http://vraidsys.com/2011/11/automatic-mcc-and-mnc-detection-using-motorola-j2me-enabled-cellphone-gsm/" title="automatic MCC and MNC detection using Motorola J2ME enabled cellphone [GSM]">automatic MCC and MNC detection app</a> I made:
<blockquote><code>MCC = 310, MNC = 260</code></blockquote>
I am able to enter all these values into the APN settings on my smartphone, but last time I tried to connect the
settings were erased. I suppose I will need to do some setprop shenanigans to re-enter the settings once the
T-Mobile tower erases them.
