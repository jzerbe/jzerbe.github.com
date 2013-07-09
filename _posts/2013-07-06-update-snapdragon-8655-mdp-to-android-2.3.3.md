---
layout: post
title: update Snapdragon 8655 MDP to Android 2.3.3
tags:
- Snapdragon
- 8655 MDP
- Android 2.3.3
- firmware update
status: publish
type: post
published: true
---
__NOTE__: IDK what happened, but after running through this I am no longer able to get an
[adb](http://developer.android.com/tools/help/adb.html) connection. Screw this!
Bought a [SGH-T959](http://forum.xda-developers.com/wiki/Samsung_Galaxy_S/SGH-T959)
and rolling CyanogenMod on the [Vibrant](http://wiki.cyanogenmod.org/w/Vibrantmtd_Info).

I bought a
[Snapdragon 8655 MDP](https://developer.qualcomm.com/mobile-development/development-devices-boards/mobile-development-devices/snapdragon-mdp-legacy-devices)
second-hand back in October 2011. I finally got access to the
[firmware updates](https://store.bsquare.com/doc_download/) at the end of Feburary 2013.
Now that I want to start using this device on a day-to-day basis,
I figured I would give the now ancient Gingerbread (API level 10) a try;
hell, it's better than the installed Froyo (API level 8).

[8655MDP_Gingerbread_Update.201107131.zip](https://docs.google.com/file/d/0B0yT30uCaFvvWnh4ZFZ5YzRvN3c/edit?usp=sharing)

###relnotes.txt###
    Release Notes for Build
    msm7630_surf-eng 2.3.3 GINGERBREAD M7630AABBQMLZA41XX_AU230 test-keys

    1) This release is applicable to devices with build number
    msm7630_1x-eng 2.2.2 FRG83G eng.androidmdp.20110224.152423 test-keys
    msm7630_1x-eng 2.2.1 FRG83 CRM1430.AU23.V01.20110105.131421 test-keys
    msm7630_1x-eng 2.2.1 FRG83 CRM1430.AU23.V01.20110111.163129 test-keys
    msm7630_1x-eng 2.2.3 GINGERBREAD M7630AABBQMLZA404005 test-keys
    Notes: To find out the Build Number of your MDP device, go to Settings --> About phone --> Build number

    2) This Package contains these files
    - boot.img
    - FlashGingerbread.bat
    - GingerbreadBL.img
    - persist.img
    - radio.img
    - readme.txt
    - recovery.img
    - relnotes.txt
    - system.img
    - userdata.img

    3) What's new
    Jul 13 2011
    - Gingerbread release
    - Trepn Profiler v1.2 is now included in the image
    - DeepSeaUI is now included in the image

    Feb 24 2011
    - DNS fixed
    - Calendar application fixed

    Jan 11 2011
    - Trepn Profiler is now included in the image

It was the `DNS fixed` item that got my attention. I would no longer have to go
through
[updating the DNS via the console](http://vraidsys.com/2011/10/android-2-2-1-custom-rooting-and-dns-not-resolving/)
every time I switched 802.11 wireless networks.

###environment setup###
0. Dependencies: [JDK](https://github.com/jzerbe/java-tool-chain-quick) 
1. Download and install the [Android SDK](http://developer.android.com/sdk/index.html).
You really only need the _SDK Tools Only_ for this procedure. Be sure to run the
SDK manager and install the platform tools. You can run this just fine on a
headless system, as long as you have _X11 Forwarding_ set up.
2. Add `NIX_SDK_INSTALL/platform-tools` or `WIN_SDK_INSTALL\platform-tools`
to the system __PATH__ or __Path__
3. Vendor ID initialization
    - Windows: `C:\Users\[current user]\.android\adb_usb.ini` should contain only the string `0x0956`.
    - Linux: `/etc/udev/rules.d/70-android.rules` should contain
        `SUBSYSTEM=="usb", ATTRS{idVendor}=="0956", MODE="0666"`.
        Sourced from: [Guide to setting up ADB for Ubuntu/Linux](http://forum.xda-developers.com/showthread.php?t=1024129)
4. 8655 MDP driver install
    - Linux: N/A
    - Windows: download+extract [msm8655-mdp_usb_driver.zip](https://docs.google.com/file/d/0B0yT30uCaFvvNjItRTl2Z25mOUE/edit?usp=sharing)
    and update drivers of unknown devices by pointing at the root of this extracted directory
5. Make sure your device is connected; `adb devices` should return a device id.

###the actual work###
Get into the extracted `8655MDP_Gingerbread_Update.201107131` directory.
Make sure the output of `fastboot getvar product` matches
`MSM7630_1x`, `MSM7630_1X`, or `MSM7630_SURF`.
The `FlashGingerbread.bat` update script did not work for me so I manually
entered the following `adb` and `fastboot` commands:

    adb reboot-bootloader

    fastboot getvar product

    fastboot erase boot
    fastboot erase system
    fastboot erase recovery
    fastboot erase persist
    fastboot erase userdata
    fastboot flash FOTA GingerbreadBL.img
    fastboot reboot

    fastboot flash FOTA radio.img
    fastboot reboot

    fastboot flash boot boot.img
    fastboot flash system system.img
    fastboot flash recovery recovery.img
    fastboot flash persist persist.img
    fastboot flash userdata userdata.img
    fastboot reboot

    adb shell "echo WCN1312 > /persist/wlan_chip_id"

Things got a bit funky, prior to initializing the `wlan_chip_id`,
when I was running this on Windows 7. Switched over to a Debian 7 box and was
still __unable__ to get an adb connection ...
