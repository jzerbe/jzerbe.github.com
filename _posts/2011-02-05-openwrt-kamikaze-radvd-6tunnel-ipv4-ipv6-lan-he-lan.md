---
layout: post
title: OpenWRT kamikaze with a HE IPv6 tunnel + LAN
tags:
- Hurricane Electric
- IPv4
- IPv6
- OpenWRT
published: true
---
I finally took the time to add IPv6 support to my LAN this weekend. Still have a long way to go: need to add
AAAA records to my LAN DNS, static IPv6 host allocations, DHCPv6, etc. I ended with the desired initial rollout:
all the dual-stacked machines on my LAN getting allocated globally routable IPv6 addresses.

Much of this is taken from the great article on OpenWRT\'s wiki:
[IPv6 HowTo on Backfire and later](http://wiki.openwrt.org/doc/howto/ipv6).
I made some specific modifications for [Hurricane Electric\'s free IPv6 tunnel broker service](http://www.tunnelbroker.net/).
This assumes you have an OpenWRT install with SSH access and that you already have an account with HE\'s tunnelbroker service.
I did this on my Actiontec GT701-WG, which cannot handle OpenWRT Backfire, hence the hackish version for Kamikaze 8.09.2.
On my specific install, I also had to stop the cron, http, and ddns services to have a comfortable amount of RAM to work
with during the install.


To use IPv6, we need the following modules:

- IPv6 kernel module: always
- routing software: always, to configure IPv6 routing
- ip6tables kernel modules: optional, if you need an IPv6 firewall
- ip6tables command-line tool: optional, to configure the IPv6 firewall

    opkg install kmod-ipv6 radvd ip kmod-ip6tables ip6tables
    opkg install 6scripts 6tunnel


First configure your firewall to allow ICMP pings to the router. This is a security risk, but if you intend to
dynamically update your IPv4 endpoint address with HE\'s automated method, you are going to need to allow pings to
your OpenWRT device. Insert the following into `/etc/config/firewall`:

    # needed for Hurricane Electric IPv6 ping probes
    config rule
    option _name    ping
    option src      wan
    option proto    ICMP
    option target   ACCEPT

Next let\'s configure the `/etc/firewall.user` script to auto-configure ip6tables for wan/br-lan devices on
start/restart/stop. Example [firewall.user](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvUW0tTlQ0NDlnbEE)
script.

Now to tie your tunnel and LAN together such that your LAN obtains globally routable IPv6 addresses, you are going to
need to get at your [Router Advertisement configuration](http://wiki.openwrt.org/doc/uci/radvd) file:
`/etc/conf/radvd`. Example [radvd conf](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvVVVleV8zaEpQZkk).
Be sure to enable radvd to start on system startup: `/etc/init.d/radvd enable`.

__UPDATE__ February 10: You are going to need to restart radvd manually each time you bring your
IPv6 tunnel up or down, so that radvd knows to broadcast either a link-local subnet or the global routed IPv6
tunnel allocated subnet.

Finally let\'s configure the tunnel itself.
Open up the 6tunnel config file - `vi /etc/config/6tunnel` - and take a look inside.
In the end you are going to want it to look like this:

    config 6tunnel
    option tnlifname        'he-ipv6'
    option remoteip4        '209.51.181.2'
    #option localip4         ''
    option localip6         '2001:470:xxxx:xxx::2/64'
    option remoteip6        '2001:470:xxxx:xxx::1/64'
    option prefix           '2001:470:xxxx:xxx::/64'
    option passmd5          'md5 hash of your tunnelbroker password'
    option userid           'UserID found on the main.php page'
    option tunnelid         'tunnel id number'

First pop open your browser to the tunnelbroker.net page _Tunnel Details_: remoteip4 = _Server IPv4 address_ on
said page, localip6 = _Client IPv6 address_, remoteip6 = _Server IPv6 address_, prefix = _Routed /64_ or
_Routed /48_ if you need something that big. localip4 will be auto-detected the customized 6tunnel init.d
script, and thus can be commented out/left blank/doesn\'t matter.

The init.d file for 6tunnel
[6tunnel init script](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvck54ZGFIRHN5TG8)
is an amalgamation of the original init script, linux-route-2 commands as suggested by HE, and inspirations from
[https://forum.openwrt.org/viewtopic.php?pid=111999#p111999](https://forum.openwrt.org/viewtopic.php?pid=111999#p111999).
This doesn\'t seem to work quite properly on system startup for me, but if you want to give it a try - `/etc/init.d/6tunnel enable` -
probably the worst that could happen is that 6tunnel times out connecting because your ADSL/Cable link is not yet up. You might need to edit
the init script\'s line #6 from `ppp0` to `eth0` or something, whatever interface your public IPv4 address is found on.
