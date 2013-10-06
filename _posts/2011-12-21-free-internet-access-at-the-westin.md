---
layout: post
title: free Internet access at The Westin
tags:
- Internet
- Westin
published: true
---
It was rather unexpected to have to pay extra for Internet access at a hotel,
especially The Westin, being all expensive and whatnot.
In the end I payed out, well the company who was paying for me payed out
($15 for 24 hrs?!?!), but had I set up a few things
before getting there, there would have been no need to pay.

I stayed at [The Westin Bellevue](http://www.starwoodhotels.com/westin/property/overview/index.html?propertyID=1555), and
would assume that many other _Starwood Hotels and Resorts_ locations must employ similar captive portal filtering. At
this particular Westin location there were three wireless networks: WestinLobby, WestinMeetingRooms, WestingGuestRooms.
They all employed the same captive portal filtering based on hostnames.

Attempting to ICMP ECHO (ping) _vraidsys.com_ would fail, but pinging _starwoodhotels.com.vraidsys.com_ worked.
Similarly an HTTP GET request to _mail.zerbe.biz_ would fail, but _starwoodhotels.com.zerbe.biz_ worked. If only
I had an SSH host for tunneling with a "valid" hostname, then there would have been no need to pay for this extortion.
