---
layout: post
title: Snapdragon 8655 MDP adb and fastboot setup on WinXP
tags:
- Snapdragon
- 8655 MDP
- Android
- adb
- fastboot
published: true
---
I finally found the full developer specs on my 8655 MDP -
[BSQUARE](http://store.bsquare.com/doc_library/docs/Snapdragon_8655_MDP_UG_rev2_0_Final_122310.pdf) or
[mirror](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvbVFLVWNWd1ZXQlU) -
along with the USB driver installation instructions -
[BSQUARE](http://store.bsquare.com/doc_library/docs/App_Note_Installing_USB_driver_on_Windows.pdf) or
[mirror](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvcEFTbVFNekhJajQ).

The USB driver installation instructions that BSQUARE published are rather out-of-date for working with version 15
of the Android SDK. This documents my current driver setup on my 32-bit Windows XP machine.

1)  [Download and Install the Android SDK](http://developer.android.com/sdk/) (v15 at time of writing)

2)  Start up the SDK Manager GUI (pop open the Tool -> Options menu and select _force https source to be
fetched over http_, had some weird errors otherwise) and install: Tools -> Android SDK Tools, Tools ->
 Android SDK Platform-tools, Extras -> Google USB Driver package
 
3) After the odiously slow process finishes, copy `[Android SDK Root]extrasgoogleusb_driver` to
`[Android SDK Root]msm8655-mdp_usb_driver`

4) Open up the `android_winusb.inf` (inside of the newly created directory) file in Notepad or
other non-format appending text editor. Clear out the contents of the `[Google.NTx86]` and
`[Google.NTamd64]` paragraphs so that they just contain the following:

    [Google.NTx86]
    ;BSQUARE MDP8655
    %SingleAdbInterface% = USB_Install, USBVID_0956&PID_9025&MI_00
    %CompositeAdbInterface% = USB_Install, USBVID_0956&PID_9025&MI_01
    %SingleBootLoaderInterface% = USB_Install, USBVID_0956&PID_9025&MI_02`
    
    [Google.NTamd64]
    ;BSQUARE MDP8655
    %SingleAdbInterface% = USB_Install, USBVID_0956&PID_9025&MI_00
    %CompositeAdbInterface% = USB_Install, USBVID_0956&PID_9025&MI_01
    %SingleBootLoaderInterface% = USB_Install, USBVID_0956&PID_9025&MI_02`

5)  Create and/or open the `C:Documents and Settings[your username].androidadb_usb.ini`
file and make sure it just contains the hex string: `0x0956`

6) Power on your device and connect it to your development machine. Be sure to point Windows to
your newly created USB driver directory for your MSM8655. If Windows is unable to find the needed driver(s),
point the dirver search to the original USB driver located in `[Android SDK Root]extrasgoogleusb_driver`.
If Windows still does not find the necessary driver just hit finish and go without. This seemed to be the case when
I rebooted into the bootloader on some occasions.

7)  Restart the SDK and see if your device is connected - `[Android SDK Root]platform-toolsadb.exe devices` -
should spit out `1234567890ABCDEF device`.

8) For a working fastboot binary for flashing, one can grab a fresh copy from the android-roms project
[[info](http://code.google.com/p/android-roms/downloads/detail?name=fastboot-win32.zip)] or the one I used for
this tutorial from my dropbox [[link](https://drive.google.com/uc?export=download&id=0B0yT30uCaFvvQWJma05NUFJfVHc)].
Once you download: unzip and move the .exe to `[Android SDK Root]platform-tools`.
