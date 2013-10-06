---
layout: post
title: CentOS interoperable Samba setup
tags:
- CentOS
- RH-Firewall
- Samba
- WiiMC
published: true
---
Contrary to what I saw in the
[HowTos/SetUpSamba - CentOS Wiki](http://wiki.centos.org/HowTos/SetUpSamba)
article, one cannot just use Windows Active Directory XOR NetBios.
Sure, if I were running a network with just Windows clients looking to
connect to Samba shares, I could only open up the NetBios ports UDP 137,138 and TCP 139.
I did this for awhile and this worked, but the response time was sluggish when
trying to connect to a Samba share from my Windows XP and 7 machines.

Then I tried this setup with WiiMC, and it just did not work. I settled on the
following firewall config that worked:

    # Firewall configuration written by system-config-securitylevel
    # Manual customization of this file is not recommended.
    *filter
    :INPUT ACCEPT [0:0]
    :FORWARD ACCEPT [0:0]
    :OUTPUT ACCEPT [0:0]
    :RH-Firewall-1-INPUT - [0:0]
    -A INPUT -j RH-Firewall-1-INPUT
    -A FORWARD -j RH-Firewall-1-INPUT
    -A RH-Firewall-1-INPUT -i lo -j ACCEPT
    -A RH-Firewall-1-INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
    
    #standard services
    -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
    -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
    ...
    #samba
    -A RH-Firewall-1-INPUT -m state --state NEW -m udp -p udp --dport 137 -j ACCEPT
    -A RH-Firewall-1-INPUT -m state --state NEW -m udp -p udp --dport 138 -j ACCEPT
    -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 139 -j ACCEPT
    -A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 445 -j ACCEPT
    -A RH-Firewall-1-INPUT -m state --state NEW -m udp -p udp --dport 445 -j ACCEPT
    
    #if none match ...
    -A RH-Firewall-1-INPUT -j REJECT --reject-with icmp-host-prohibited
    
    COMMIT


And the smb.conf:

    [global]
    interfaces = eth1
    bind interfaces only = yes #bind only to above interfaces
    workgroup = G-UNIT
    server string = Samba Server Version %v
    netbios name = BIGBOY
    local master = yes
    os level = 34 #be sure to become master browser
    preferred master = yes
    security = share
    
    [public-rw]
    path = /misc/wd500/public-rw #auto.misc
    public = yes
    browseable = yes
    guest ok = yes
    read only = no
    
    [public-r]
    path = /misc/wd500/public-r #auto.misc
    public = yes
    browseable = yes
    guest ok = yes
    read only = yes
