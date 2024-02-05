---
title: "PS2 Free McBoot (FMCB)"
date: 2020-05-30 22:26:02
lastmod: 2020-06-07 20:49:47
tags:
- tutorial
---

![fmcblogo](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTgYFR-shJoyTVQxBeHIXMV8y4LOHRUPBbd-w&usqp=CAU)

Aku mengidam nak main balik *Grand Theft Auto: San Andreas* dekat Playstation 2. Sekali, bila cari CD game yang aku kumpul sejak zaman sekolah dulu dah hilang. Nak beli koleksi CD game dekat Carousell mahal pula dan kadang-kadang tiada game yang aku nak main. Mujur terjumpa mod FMCB ini.

- [Apa itu Free McBoot?](#apa-itu-free-mcboot)
- [Prasyarat](#prasyarat)
- [Muat turun game](#muat-turun-game)
- [Mula Bermain](#mula-bermain)
  - [Main dengan CD (DVD-R)](#main-dengan-cd-dvd-r)
  - [Main dengan Internal HDD](#main-dengan-internal-hdd)
  - [Main dengan USB thumb drive](#main-dengan-usb-thumb-drive)
- [Barang-barang](#barang-barang)
  - [Windows (PC)](#windows-pc)
  - [Aplikasi (FMCB)](#aplikasi-fmcb)
  - [Hardware](#hardware)
- [Apendiks](#apendiks)
  - [Mengemaskini firmware FMCB](#mengemaskini-firmware-fmcb)
  - [Mengemaskini dan menambah aplikasi ke dalam FMCB](#mengemaskini-dan-menambah-aplikasi-ke-dalam-fmcb)
  - [Membuat gantian FMCB ke memory card lain](#membuat-gantian-fmcb-ke-memory-card-lain)
- [Sumber](#sumber)

# Apa itu Free McBoot?

Dibina oleh Jimmikaelkael dan Neme pada tahun 2008. Ia adalah eksploit memory card Playstation 2 yang membolehkan kita untuk memulakan program FMCB. FMCB ini tidak mengandungi game, ia cuma aplikasi boot loader yang menggantikan menu default Playstation 2. Banyak aplikasi yang dibina oleh manusia-manusia banyak masa kelapangan untuk kesenangan kita bersama. Sebagai contoh, emulator Nintendo, graphics upscalers, file explorer, dan lain-lain. Dengan ada nya FMCB loader ini, kita dapat main game PS2 yang dimuat turun dari internet melalui CD, USB thumb drive dan juga internal HDD. *([sumber](https://www.ps2-home.com/forum/viewtopic.php?t=1248))* *([sejarah](https://www.ps2-home.com/forum/viewtopic.php?t=6956))*

# Prasyarat

1. **Konsol Playstation 2 *(unmodded)***

   Tak semua model konsol Playstation 2 menyokong FMCB boot loader ini. Model konsol yang aku guna adalah *SCPH-39006* (ia menyokong FMCB boot loader), jadi aku tak berapa kisah untuk nak tahu model yang lain untuk pos ini. Sesetengah model slim PS2 **tidak** menyokong FMCB boot loader.<br/><br/>
   
   Bacaan teknikal terperinci ada di [sini](https://www.ps2-home.com/forum/viewtopic.php?t=5341),
   atau terus lihat carta model compatibility di [sini](https://www.ps2-home.com/forum/aplikasi.php/page/fmcb-compatible-ps2-models-chart).

   ![Console Model][console-model]  
   <small>*Model konsol dibelakang unit PS2*</small><br/><br/>

2. **FMCB memory card**

   Selain daripada melakukan [modchip hack](https://www.youtube.com/watch?v=QieLwx_6Z6I) atau cara boot lain, cara paling mudah adalah dengan menggunakan memory card yang sudah ada boot loader FMCB. Ya, benar, kita main game lanun hanya dengan menyisipkan memory card ke dalam Playstation 2.

   ![FMCB memory card][fmcb-memory-card]  
   <small>*Memory card yang telah dimuatkan FMCB*</small><br/><br/>

   FMCB memory card boleh didapati dengan harga yang murah (~RM30), di pasaran dalam talian kegemaran: [Shopee](https://shopee.com.my/product/181059869/3915274989), Lazada atau Ebay. Elok beli saiz yang paling besar sebab kita dah dewasa, berduit, tak perlu tamak untuk mengeluarkan tambahan RM10 untuk tambahan storan memory card ini.

   ![FMCB menu][fmcb-menu]  
   <small>*Menu FMCB loader v1.953*</small><br/>

# Muat turun game

Ada banyak website yang menyajikan ISO image game PS2 diluar [sana](https://lmgtfy.com/?q=download+game+ps2+iso). Purata saiz file image ini dalam sekitar 1-5 GB.

- http://www.portalroms.com/en/isos/ps2
- http://coolrom.com/roms/ps2/
- https://romsmania.com/roms/playstation-2
- https://cdromance.com/ps2-iso/
- http://romhustler.net/roms/ps2
- https://www.romulation.net/roms/PS2

Adalah **disarankan** (tapi optional), untuk membandingkan file hash `.iso` yang telah dimuat turun dengan file hash compatibility ESR/OPL dalam senarai yang dibina oleh komuniti ini (lihat ruangan `Notes/MD5`):

1. [OPL compatible games][opl-compatible]
2. [ESR compatible games][esr-compatible]

Ini adalah untuk memastikan game yang telah dimuat turun itu tidak korup atau mengandungi bahan-bahan yang berniat jahat (virus, malware, dll.).

Semak MD5 file hash menggunakan PowerShell (Windows) dan bandingkan dengan yang dalam senarai komuniti diatas:

```bash
$ Get-FileHash <filepath.iso> -Algorithm MD5
```

# Mula Bermain

Ada beberapa cara yang biasa orang pakai untuk mula bermain game, mengikut kesenangan masing-masing. Aku sarankan untuk mula bermain dengan CD agar tidak hampa dengan kegagalan pertama bila bermain dengan USB thumb drive atau HDD.

Antara sebabnya adalah kalau guna thumb drive, game akan tersekat ditengah permainan atau tidak akan load langsung atas sebab tertentu, dan pindahan data yang perlahan menggunakan USB port yang lama (USB version [1.1](https://en.wikipedia.org/wiki/PlayStation_2_technical_specifications#Connectivity)). Jadi, lebih baik jika guna CD atau kalau ada bajet yang tinggi sikit, guna HDD.

> "You have a fat ps2, so there is no reason to use a USB hard drive with it. Internal hard drive has the fastest data speeds, while USB has the slowest, which is slower than CD or DVD."
> &mdash; [Pengguna Reddit](https://www.reddit.com/r/ps2/comments/60t76g/how_to_install_games_on_hdd/df9we3b/)

Kaedah permainan:

1. [CD (DVD-R)](#main-dengan-cd-dvd-r) (ESR)
1. [Internal HDD](#main-dengan-internal-hdd) (OPL)
1. [USB thumb drive](#main-dengan-usb-thumb-drive) (OPL)

## Main dengan CD (DVD-R)

Senarai game yang disokong oleh ESR boleh dilihat [disini][opl-compatible].

1. Dapatkan DVD-R kosong &mdash; kebiasaannya berkapasiti 4.7GB, boleh memuatkan satu game sahaja
2. Guna [ESR Disc Patcher GUI][esr-disc-patcher] untuk patch `.iso` image yang telah dimuat turun
   - Tanpa patcher ini, game tidak dapat dimuatkan (!)
   - Maklumat lebih lanjut mengenai [ESR](https://www.ps2-home.com/forum/viewtopic.php?f=10&t=15)
3. Guna aplikasi [imgburn](http://imgburn.com/) untuk burn `.iso` image kedalam DVD tersebut 
   - Boleh juga menggunakan built-in burner oleh Windows/Linux untuk melakukan proses ini
4. Masukkan CD yang telah di bakar ke dalam talam CD Playstation 2
5. Mula bermain menggunakan **ESR** aplikasi di dalam FMCB menu

![Burned DVD][burned-dvd]  
<small>*DVD yang telah di-burn*</small><br/><br/>

## Main dengan Internal HDD

Untuk main dengan HDD, kau perlu ada 1) Network Adapter interface yang menyokong sambungan SATA/IDE dan 2) ternyata sekali, sebiji hard disk (SATA/IDE).

HDD ini tak kisah berapa saiz, ikut objektif kau nak letak berapa game dalam tu. Benda ini disambungkan belakang konsol iaitu dibahagian *Expansion Bay* Playstation 2. Network Adapter ini boleh dibeli di pasaraya [dalam talian](https://www.lazada.com.my//products/i519022724-s1003944221.html?spm=a2o4k.cart.0.0.3cba49fbyTFyst&urlFlag=true) berharga sekitar RM50 *(bukan asli)*. Ironi nya, nama sudah Network Adapter, tapi tiada ethernet port. Ini kerana ia tidak asli oleh Sony lol.

![SATA/IDE network adapter][sataide-network-adapter]

<span style="color: red;">**(Harus diingatkan!)**</span> Bahawa ada sesetengah manufacturer/model HDD sahaja yang menyokong boot loader ini. Senarai sokongan boleh dilihat disini:

- [List of Compatible USB HDDs for OPL and POPStarter (PS2)][usb-hdd-compatibile]
- [HDD compatibility][ps2drive-hdd-compatible]

Berikut adalah langkah-langkah untuk main dengan internal HDD:

*Diambil daripada tutorial video bersama Tech James: https://www.youtube.com/watch?v=tvEySPbWxZc*

1. *(Optional)* Guna [OPL Manager][opl-manager] aplikasi untuk mengemaskini info game (title, description, cover art, game id, dll.)
2. Guna tool [WINHIIP][winhiip] (run as administrator) untuk format HDD ke Playstation 2 format
   - Options > tanda kan pilihan berikut:
     - Enable 48bit HDLoader &#9745;
     - Reduce Fragmentation &#9745;
     - Log File Generation &#9745;
     - Enable Patched HDL's Extended Compatibility Modes &#9745;
3. Guna tool yang sama (WINHIIP) untuk memasukkan image `.iso` ke dalam HDD dengan pilihan `Add Image(s)`
4. Sambungkan HDD ke dalam *Expansion Bay* menggunakan Network Adapter
5. Mula bermain dengan **OPL** aplikasi di dalam FMCB menu

*Info lebih lanjut mengenai penggunaan [WINHIIP](https://www.ps2-home.com/forum/viewtopic.php?t=168/).*

## Main dengan USB thumb drive

Senarai game yang disokong oleh OPL (Open PS2 Loader) boleh dilihat [disini][opl-compatible]. Juga, ambil perhatian di ruang `Notes/MD5` list tersebut kerana ia mengandungi maklumat tentang MD5 hash dan modes yang diperlukan untuk memuatkan game melalui aplikasi OPL ini.

Thumb drive yang akan digunakan ini hendaklah diformatkan ke **FAT32** terlebih dahulu. Fahami lah bahawa, PS2 ini sistem legasi, maka semua teknologi yang digunakan adalah dari zaman 2000-an.

**Untuk image file yang bersaiz *kurang* dari 4GB.**

1. Masukkan thumb drive ke dalam port USB PC
2. Buat folder baru di dalam root drive bernama `/dvd`
3. Masukkan `.iso` image ke dalam folder tersebut
4. Mula bermain dengan **OPL** aplikasi di dalam FMCB menu

Jika ada kegagalan dalam percubaan pertama, cuba tukar mode di dalam game settings (tekan <span style="color: green;">&#9650;</span>). Info lanjut mengenai [OPL modes](https://www.ps2-home.com/forum/viewtopic.php?f=50&t=182). Lihat juga senarai [OPL game compatibility][opl-compatible] di bawah ruangan `Notes/MD5` jika ada nota mengenai mode yang perlu dipakai.

**Untuk image file yang bersaiz *lebih* dari 4GB.**

Langkah yang sama seperti diatas, cuma sebelum `.iso` image dimasukkan, lakukan langkah ini:

1. Guna aplikasi [USBUtil 2.0][usb-util] untuk menceraikan `.iso` image kepada pecahan yang kecil &mdash; sebab FAT32 format tidak menyokong saiz fail [melebihi 4GB](https://answers.microsoft.com/en-us/windows/forum/windows_7-files/what-is-the-maximum-file-size-fat-fat32-ntfs-file/1663db6b-490e-4021-9e36-f7a6976ac0c0).
2. Pecahan kecil ini haruslah diletakkan di dalam root drive (`X:/`), **bukan** di dalam folder `/dvd`
3. Mula bermain dengan **OPL** aplikasi di dalam FMCB menu

Jika ada masalah untuk memuat game ke dalam OPL, cuba pendekkan title game tersebut di dalam settings USBUtil tadi. Seperti biasa, rujuk [OPL game compatibility][opl-compatible] untuk menggunakan mode lain.

*Tutorial video bersama Project Phoenix Media: https://www.youtube.com/watch?v=MWwUX3que7w*

# Barang-barang

Berikut adalah senarai aplikasi yang disebut dalam pos ini, dan juga aplikasi yang biasa aku guna untuk catatan persendirian. 

## Windows (PC)

1. [USBUtil][usb-util] &mdash; Proses penceraian `.iso` image berlaku disini.
2. [Imgburn](http://imgburn.com/) &mdash; Proses membakar `.iso` image ke dalam DVD-R berlaku disini.
3. [ESR Disc Patcher GUI][esr-disc-patcher] &mdash; Proses yang membolehkan kita main `.iso` image dalam PS2 berlaku disini.
4. [WINHIIP][winhiip] &mdash; Proses mengformat dan memasukkan `.iso` image ke dalam HDD berlaku disini.
5. [OPL Manager][opl-manager] &mdash; Proses mengemaskini info game berlaku disini.

## Aplikasi (FMCB)

1. **Open PS2 Loader (OPL)** &mdash; Muat game melalui internal HDD, SMB (network), atau mass storage (USB thumb drive). *([Bebenang](https://www.ps2-home.com/forum/viewtopic.php?f=13&t=150))*
2. **ESR** &mdash; Muat game melalui CD. *([Bebenang](https://www.ps2-home.com/forum/viewtopic.php?f=10&t=15))*
3. **uLaunchELF (uLE)** &mdash; PS2 file manager untuk kerja format-an dan kemaskini firmware FMCB. *([Bebenang](https://www.ps2-home.com/forum/viewtopic.php?f=16&t=1895))*
4. **Simple Media System (SMS)** &mdash; Aplikasi FMCB untuk memainkan media seperti MP3, video file, dll. *([Bebenang](https://www.ps2-home.com/forum/viewtopic.php?f=14&t=18))*

Banyak lagi aplikasi yang boleh dimuatkan ke dalam FMCB menu. Berikut adalah [senarai][fmcb-apps] nya. Panduan untuk mengemaskini atau menambah aplikasi ini akan dilanjutkan di bahagian [Apendiks](#apendiks).

## Hardware

5. Playstation 2
6. FMCB memory card
1. SATA/IDE Network Adapter
2. SATA/IDE hard drive
3. USB thumb drive
4. DVD-R (4.7GB)

# Apendiks

## Mengemaskini firmware FMCB

TODO

***Nota:** Tak penting. Mana-mana versi FMCB di atas v1.9x sudah memadai.*

Lanjut: https://www.ps2-home.com/forum/viewtopic.php?f=11&t=1890

## Mengemaskini dan menambah aplikasi ke dalam FMCB

TODO

***Nota:** Agak penting. Sesetengah FMCB memory card yang dibeli dalam talian (atau kemaskini FMCB ke versi terbaru) tidak mengandungi aplikasi penting seperti OPL dan ESR.*

Tutorial: https://www.youtube.com/watch?v=kqM3p5u4ojg

## Membuat gantian FMCB ke memory card lain

Ia tidak mustahil.

Ia berguna jika ada memory card lebihan. Macam aku, ada FMCB memory card (64MB, v1.953) dan memory card lama Kemco (8MB, v1.966). Satu aku buat sebagai backup (8MB), dan satu lagi (64MB) digunakan untuk sebagai memory card utama aku.

Ini adalah penting jika berlaku sebarang kerosakkan terhadap memory card yang mengandungi boot loader FMCB, kau ada satu lagi. Apa yang penting sekarang adalah boot loader tersebut.

![FMCB dan backup][fmcb-backup]  
<small>*Memory card FMCB dan backup*</small><br/><br/>

1. Dengan syarat, salah satu memory card ini ada FMCB boot loader, gunakan aplikasi **uLaunchELF** untuk proses salinan antara dua memory card ini.
2. Salinkan segala kandungan dalam memory card yang mengandungi FMCB, masuk ke dalam memory card lagi satu
3. Simpan yang gantian ini ditempat yang selamat untuk digunakan dikala berlaku kecelakaan hidup.

# Sumber

Console model compatibility
- https://www.ps2-home.com/forum/viewtopic.php?t=5341
- https://www.ps2-home.com/forum/aplikasi.php/page/fmcb-compatible-ps2-models-chart

OPL game compatibility
- https://www.ps2-home.com/forum/page/opl-game-compatibility-list-1

ESR game compatibility
- https://www.ps2-home.com/forum/page/esr-game-compatibility-list-1

USB/HDD compatibility
- https://www.ps2-home.com/forum/aplikasi.php/page/ps2-usb-hdd-compatibility-list-with-opl-and-popstarter
- https://ps2drives.x-pec.com/?p=list

OPL troubleshoot
- https://www.ps2-home.com/forum/viewtopic.php?f=50&t=176

Sejarah Free McBoot dan bagaimana ia berfungsi
- https://www.ps2-home.com/forum/viewtopic.php?t=6956

Forum
- https://ps2-home.com/forum
- https://www.psx-place.com/

[esr-disc-patcher]: https://www.mediafire.com/file/224im87yskuhtcn/ESR_disc_patcher_GUI_v0.24a.zip/file
[usb-util]: https://www.mediafire.com/file/d7evrq6p3w5gd6q/USBUtil_v2.0_Full_%28English%29.zip/file
[opl-manager]: https://www.mediafire.com/file/msb7fzqq3o0tjfs/OPL_Manager_V21.6.zip/file
[winhiip]: http://www.mediafire.com/file/obtulryx3sqcksr/WinHIIP_V1.7.6.rar/file
[opl-compatible]: https://www.ps2-home.com/forum/page/opl-game-compatibility-list-1
[esr-compatible]: https://www.ps2-home.com/forum/page/esr-game-compatibility-list-1
[fmcb-apps]: https://www.ps2-home.com/forum/search.php?keywords=&terms=all&author=&attr_id=1&sc=1&sf=all&sr=topics&sk=i&sd=a&st=0&ch=-1&t=0&submit=Search#
[usb-hdd-compatibile]: https://www.ps2-home.com/forum/aplikasi.php/page/ps2-usb-hdd-compatibility-list-with-opl-and-popstarter
[ps2drive-hdd-compatible]: https://ps2drives.x-pec.com/?p=list
[console-model]: https://i.imgur.com/dMFMem3.jpg
[fmcb-memory-card]: https://i.imgur.com/uFSdr2B.jpg
[fmcb-menu]: https://i.imgur.com/vfReSj9.jpg
[sataide-network-adapter]: https://i.imgur.com/AWiGv2Y.jpg
[fmcb-backup]: https://i.imgur.com/SktShpY.jpg
[burned-dvd]: https://i.imgur.com/sDMV6nW.jpg
