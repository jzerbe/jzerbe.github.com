---
layout: post
title: ubuntu (11.04) alternate install over local network
tags:
- Ubuntu
published: true
---
Digging through the Ubuntu help docs, specifically "<a href="https://help.ubuntu.com/community/Installation/LocalNet">InstallationLocalNet</a>",
today was rather frustrating. There was not one specific (modern) way given, in said documentation: they illustrated using
<a href="http://en.wikipedia.org/wiki/Bootstrap_Protocol">BOOTP</a>?!?! That said, time to make this work using (i)PXE.<br />
<br />
The ubuntu alternate install discs do not have the <a href="http://www.linuxcertif.com/man/7/casper/">casper live system</a>
which allows for the disc files to be pulled over NFS or CIFS (Samba). Although, with the existing preseed framework
(extended from Ubuntu's parent, Debian), one can pull from a local network package repository (the expanded disc image files).<br />
<br />
I use <a title="network boot without a proper DHCP setup for PXE" href="http://vraidsys.com/2011/06/network-boot-without-a-proper-dhcp-setup-for-pxe/">iPXE for various reasons</a>,
instead of the generic PXE setup involving TFTP and a complicit DHCP server, which is able to use HTTP and DHCP itself
without the niceties of a LAN where you have access to the DHCP boot options.<br />
<br />
<strong>my iPXE boot options for ubuntu studio 11.04:</strong><br />
<blockquote><code>kernel ubuntustudio-11.04-alternate-i386/install/netboot/ubuntu-installer/i386/linux</code><br />
    <code>initrd ubuntustudio-11.04-alternate-i386/install/netboot/ubuntu-installer/i386/initrd.gz</code><br />
    <code>imgargs linux append auto url=http://&lt;?php echo $server_ip; ?&gt;/tftp/boot/preseed_ubuntustudio.cfg
        language=en country=US console-keymaps-at/keymap=us keyboard-configuration/xkb-keymap=us
        keyboard-configuration/layoutcode=us console-setup/ask_detect=false --</code></blockquote>
<br />
<strong>contents of my custom preseed_ubuntustudio.cfg:</strong><br />
<blockquote><code>#Debian 6 options<br />
        d-i debian-installer/locale string en_US<br />
        d-i console-keymaps-at/keymap select us<br />
        d-i keyboard-configuration/xkb-keymap select us<br />
        d-i netcfg/choose_interface select auto<br />
        <br />
        #Ubuntu Natty specific options</code><br />
    <code># ubuntustudio.seed linked from ubuntustudio-11.04-alternate-i386/preseed/ubuntustudio.seed<br />
        d-i preseed/include string ubuntustudio.seed<br />
        # Installation source<br />
        d-i mirror/country string manual<br />
        # hostname This is whatever HTTP server you have set up.<br />
        d-i mirror/http/hostname string bigboy<br />
        # This is the /ubuntu directory from the install CD copied (or linked) under the webroot of your HTTP server.
        Must be "/ubuntu" otherwise this will not work.<br />
        d-i mirror/http/directory string /ubuntu<br />
        # Name your ubuntu version here<br />
        d-i mirror/suite string natty<br />
        # no proxy<br />
        d-i mirror/http/proxy string</code></blockquote>
<br />
I am able to use the above after extracting the contents of the <a href="http://ubuntustudio.org/downloads">ubuntustudio 11.04 iso</a>
to a location (the ubuntustudio-11.04-alternate-i386 directory) on my fileserver accessible from my HTTP server. Now I am able
to do an entirely network based boot from the contents of the ubuntu alternate disc.
