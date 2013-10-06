---
layout: post
title: power button does nothing on bare CentOS 5.8 install
tags:
- ACPId
- CentOS
- power
- button
published: true
---
Bare CentOS 5.8 install over the weekend.

When I mean bare, I mean disabling all packages in the installer;
just linux kernel, init scripts, bash, networking.
I had no idea that this would mean no ACPId ... which meant nothing
happened when I pushed the power button.


###the package fix

    yum install acpid


###the config file

`/etc/acpi/events/power.conf`

    # ACPID config to power down machine if powerbutton is pressed, but only if
    # no gnome-power-manager is running
    event=button/power.*
    action=/sbin/poweroff
