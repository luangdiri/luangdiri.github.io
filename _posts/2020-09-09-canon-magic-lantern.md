---
title: "Canon Magic Lantern"
date: 2020-09-09 00:47:28
lastmod: 2020-09-23 03:10:55
tags:
- tutorial
---

**Table of contents:**
- [Caution and Cons](#caution-and-cons)
- [Pros](#pros)
- [Installing/Uninstalling](#installinguninstalling)
  - [Installation](#installation)
  - [Uninstallation](#uninstallation)
- [Footnotes](#footnotes)
  - [Some Interesting Links](#some-interesting-links)
  - [Software](#software)
  - [My Magic Lantern Journey](#my-magic-lantern-journey)

[Download][3] Magic Lantern firmware.

Read [user guide][7] (old) and [camera guide][2].

Read installation [wiki][1].

### Caution and Cons

Before proceeding, 

- **DYOR:** [Reddit][4], [Forum][5], [IRC][6]
- Warranty might void
- Camera might permanently brick
- Magic Lantern **does not** work with an SD card that does not have the firmware inside:
  - Firmware re-installation must be done with each new SD card
- **DO NOT** format the SD card (just to be safe)
  - Only move the files from DCIM to another place if you want to clear some storage
- **DO NOT** remove the Magic Lantern firmware from the SD card, leave it inside if you plan to use it forever
  - Best to use USB cable to connect to a laptop
- After opening the card door, **always wait** for LED confirmation (or for 5 seconds) before removing the card, even if your camera is turned off

### Pros

- Magic Lantern works without flashing the internal camera ROM
- Magic Lantern runs directly from the SD card itself, hence *point #4* in [Caution and Cons][8]

### Installing/Uninstalling

#### Installation

**Installing video tutorial:** https://www.youtube.com/watch?v=BhQf-pbrFQs

*Install correct firmware (downgrade/upgrade):*

1. Format SD in-camera settings
2. Insert SD into laptop
3. Copy correct Canon Firmware into SD
4. Insert SD into camera
5. Update firmware in menu settings

*Install Magic Lantern firmware:*

0. Turn off camera, take out SD
1. Insert SD into laptop
2. Copy Magic Lantern firmware into SD
3. Insert SD into camera
4. Update firmware in menu settings

At the **"Please restart your camera"** screen, turn off your camera, then turn it on again.

Press `Erase` button to access Magic Lantern menus.

**Backup previous ROM**

1. Turn off camera, take out SD
2. Insert SD into laptop
3. *(ROM backup)*: In `./ML/Log` folder, copy `.bin` files into computer

#### Uninstallation

1. Use the SD with the Magic Lantern firmware inside
2. Format the SD in-camera settings
3. Refer to point (ii) below:

*From Magic Lantern [wiki][1]:*

> (i) From camera:
> 1. Run Firmware Update from your Magic Lantern card.
> 2. Follow the on-screen instructions, including the fine print.
> 
>     This procedure disables the `BOOTDISK` flag. You will no longer be able to run Magic Lantern, unless you reinstall it.
> 
>     Some settings changed by Magic Lantern might be persistent; this procedure will not reset them. To restore the camera to factory state, you may also want to clear all camera settings and custom functions from Canon menu.
> 
> (ii) From one card:
> 1. Canon menu: Format card, remove Magic Lantern.
> 2. This procedure does not disable the `BOOTDISK` flag. With this method, you will still be able to run Magic Lantern from other cards.
> 3. Never delete the Magic Lantern files from the card! Format the card instead.

### Footnotes

#### Some Interesting Links

- Videographers features -- https://www.youtube.com/watch?v=huj3LOTyG0A
- General features -- https://www.youtube.com/watch?v=9eYHRDKViDE
- Dual ISO mode -- https://www.magiclantern.fm/forum/index.php?topic=7139 ([Lightroom plugin][9])
- Color grading script -- https://www.magiclantern.fm/forum/index.php?topic=7022
- Intervalometer ramping -- https://www.magiclantern.fm/forum/index.php?topic=8431.0
- Focus stacking -- https://www.youtube.com/watch?v=CcRVXuc6Yt4
- Extreme ETTR processing -- http://photography.grayheron.net/2020/01/extreme-ettr-processing.html
- Post-processing workflow -- https://www.magiclantern.fm/forum/index.php?board=14.0
- ETTR + Deflicker -- https://www.youtube.com/watch?v=jgjFeYjbXmY

#### Software

In case I forgot which software I occasionally use in my workflow:

- This post -- [Magic Lantern][3]
- Image editor -- Photoshop CS6
- Image editor (RAW) -- Lightroom Classic CC
- Video converter -- [HandBrake][10]
- Create startrail-images and timelapse movies -- [Startrails.exe][11]
- Video encoder -- [Avidemux][13]

#### My Magic Lantern Journey

<script async src="https://telegram.org/js/telegram-widget.js?11" data-telegram-post="film_roll/203" data-width="100%"></script>

[1]: https://wiki.magiclantern.fm/install
[2]: https://wiki.magiclantern.fm/camera_help
[3]: https://builds.magiclantern.fm/
[4]: http://www.reddit.com/r/MagicLantern/
[5]: http://www.magiclantern.fm/forum/
[6]: http://webchat.freenode.net/?channels=magiclantern
[7]: https://wiki.magiclantern.fm/userguide
[8]: https://aemxn.xyz/notas/magic-lantern/#caution-and-cons
[9]: https://www.magiclantern.fm/forum/index.php?topic=11056.0
[10]: https://handbrake.fr/
[11]: https://www.startrails.de/
[13]: http://avidemux.sourceforge.net/
