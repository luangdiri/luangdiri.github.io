---
title: "Samsung Galaxy SII I9100G: cMIUI ROM Flashing Guide"
created: 2015-01-29 04:33:14
updated_at: 2015-01-29 04:33:14
tags:
- tutorial
---

*First posted on 23/02/2013*

I'm going to demonstrate on how to root and flash a custom ROM into Samsung Galaxy SII I9100G. This guide can be applied to any other custom ROM available for SGSII. Note that this is only an example of how to flash a custom ROM.

Before you do anything to your phone, I advise that you backup all of the apps and its data, and the system's data. Use Titanium Backup (Rooted) for backup.

Happy flashing! (pun intended)

#### Please follow these steps for a clean install custom ROM

- Flash stock ROM with Odin3
- Root your stock ROM by flashing latest CWM and Root.zip
- Flash your desired custom ROM

### 1. Flashing with Odin3 + Download Mode (Clean install)

1. Download [Odin3](http://www.android.gs/download-odin-1-85/). I'm using v1.85
2. Download custom ROM (.tar.md5) in this case I'll be using [I9100GXXLSR_I9100GXEFLSR_XEF.zip](http://forum.xda-developers.com/showthread.php?t=1679636) | 565.9 MB (France JellyBean Stock ROM)
3. Open Odin3. Do NOT touch any of the Option.
4. In Files [Download] section, press PDA button and choose .tar.md5 ROM file that you have downloaded.
5. Now reboot your phone to Download Mode (volume DOWN key + home button + power buttons), Press volume up to proceed.
6. Plug in phone cable into a USB port on your computer. Odin3 will recognize your phone automatically.
7. Press Start to start flashing.
8. Wait until success message appears and your phone will be rebooted automatically.
9. There you have it, a new stock Jelly Bean ROM installed into your phone.

### 2. Rooting process (Install ClockWorkMod (CWM) and Root.zip)

1. Before flashing any custom ROM with recovery mode, you need to have decent ClockWorkMod (CWM) Recovery tool installed inside your phone first.
2. Download the latest LP7 ClockworkMod Recovery (Mine is [GT-I9100G_LP7_ClockworkMod-Recovery_5.5.0.4.tar](lp7)) and do the flashing with Odin3, same procedure as the first tutorial section.
3. Download [Root.zip](root). This will give you a root app called SuperUser and root your phone. The procedure is as the same as the one below. *YOU MUST DO THIS STEP FIRST AFTER FLASHING CWM*.

### 3. Flashing with Recovery Mode (cMIUI ROM)

1. Download [cMIUI 2.10.19](http://xdafileserver.nl/index.php?dir=Samsung%2FGalaxy+S+II%2FTeamXD%2FcMIUI) (.zip, 182 MB) do NOT extract.
2. Copy .zip file into external SD memory or internal memory.
3. Reboot to Recovery Mode (volume UP key + home + power button), navigate by using volume buttons. Power button as enter.
4. Choose wipe data/factory reset and wipe cache partition. Do not worry, this will erase factory data only.
5. Choose Install from .zip. Depends on where you copied the .zip file (external or internal), select the .zip that you have downloaded earlier.
6. Choose Yes.
7. Follow the instruction given by the installer *(That fancy installer interface is called AROMA)*
8. You should not get any error message if you have successfully installed the ROM.
9. Reboot your phone and enjoy cMIUI custom ROM.
10. Do rooting again to root cMIUI. Use the previous Root.zip

### Footnotes:

If you come from another custom ROM, it is advisable that you have a full wipe of your system first before flashing any new ROM.

It is better if you flash a stock ROM and then you will be able to flash a new ROM (clean install)

Remember, **volume UP is for Recovery Mode**, **volume DOWN is for Download Mode** followed by Home button and power button.

If you bricked your phone (either one these step fails), there are several things that you can do to that might fix the problem:

1. Wipe data/factory reset and Wipe cache partition
2. Keep calm, turn your phone off, remove the battery for about a minute and do a clean stock ROM install.
3. If problem persists, consider downloading a suitable ROM for your type of phone. If your Samsung is an I9100 non-G variant, then download the appropriate ones. This tutorial is for G variant.
4. Else, please seek experts.

### Source

Online support community
[http://forum.xda-developers.com/](http://forum.xda-developers.com/)

Fix an unflashable or soft bricked i9100g
[http://forum.xda-developers.com/showthread.php?t=1619571](http://forum.xda-developers.com/showthread.php?t=1619571)

Galaxy SII I9100G ROM/KERNEL List
[http://forum.xda-developers.com/showthread.php?t=1588737](http://forum.xda-developers.com/showthread.php?t=1588737)
