---
title: "PS4 Jailbreak (GoldHEN, Homebrew & FPKG)"
date: 2026-11-01 15:39:50
lastmod: 2026-11-01 15:39:55
tags:
- tutorial
---

<img src="https://i.imgur.com/DLbTNUV.png">

- [Apa itu PS4 Jailbreak?](#apa-itu-ps4-jailbreak)
  - [Apa itu GoldHEN?](#apa-itu-goldhen)
- [Firmware Compatibility](#firmware-compatibility)
- [Prasyarat](#prasyarat)
- [Mula Jailbreak](#mula-jailbreak)
  - [Konfigurasi Network dan Custom DNS](#konfigurasi-network-dan-custom-dns)
    - [Cara menetapkan Custom DNS](#cara-menetapkan-custom-dns)
    - [Penting untuk pengguna firmware 13.50](#penting-untuk-pengguna-firmware-1350)
  - [Kaedah 1: Browser-based Jailbreak](#kaedah-1-browser-based-jailbreak)
  - [Kaedah 2: PPPwn](#kaedah-2-pppwn)
- [Memasang Homebrew Store](#memasang-homebrew-store)
  - [Cara pemasangan](#cara-pemasangan)
- [Memasang Game dalam Format PKG / FPKG](#memasang-game-dalam-format-pkg--fpkg)
  - [Cara memasang package melalui USB](#cara-memasang-package-melalui-usb)
  - [Susunan pemasangan game](#susunan-pemasangan-game)
    - [Contoh: Star Wars Jedi: Survivor (CUSA33778)](#contoh-star-wars-jedi-survivor-cusa33778)
  - [Memasang package melalui rangkaian](#memasang-package-melalui-rangkaian)
- [Aplikasi dan Utility Berguna](#aplikasi-dan-utility-berguna)
  - [1. Apollo Save Tool](#1-apollo-save-tool)
  - [2. Homebrew Store](#2-homebrew-store)
  - [3. Cheats Manager](#3-cheats-manager)
  - [4. PS4-Xplorer](#4-ps4-xplorer)
  - [5. Itemzflow](#5-itemzflow)
  - [6. RetroArch](#6-retroarch)
  - [7. Orbis Toolbox](#7-orbis-toolbox)
  - [8. FTP Server](#8-ftp-server)
- [Apendiks](#apendiks)
  - [Mengelakkan System Update](#mengelakkan-system-update)
  - [Backup Save Game](#backup-save-game)
  - [Kernel Panic dan Restart](#kernel-panic-dan-restart)
  - [Rest Mode](#rest-mode)
  - [Port Download Game](#port-download-game)
  - [Sumber dan Rujukan](#sumber-dan-rujukan)


Sejak kebelakangan ini aku mula berminat untuk mengubah suai PlayStation 4 supaya boleh menggunakan pelbagai aplikasi homebrew. Kalau zaman PS2 dahulu kita menggunakan Free McBoot (FMCB), untuk PS4 pula komuniti mempunyai beberapa kaedah jailbreak seperti WebKit Exploit, PPPwn dan GoldHEN.

Jailbreak PS4 ini membuka banyak kemungkinan. Antaranya memasang aplikasi homebrew, menggunakan emulator, mengurus save game, memasang mod dan cheat, menggunakan file manager serta menjalankan pelbagai utiliti yang tidak disediakan oleh Sony.

Dalam panduan ini aku akan menggunakan PS4 dengan firmware 13.50 sebagai rujukan. Aku juga akan senaraikan beberapa aplikasi yang berguna untuk memudahkan penggunaan PS4 yang telah dijailbreak.

## Apa itu PS4 Jailbreak?

Jailbreak ialah proses mengeksploitasi kelemahan keselamatan sistem operasi PS4 (Orbis OS) untuk mendapatkan akses yang lebih tinggi daripada pengguna biasa.

Secara ringkas, ia membolehkan kita menjalankan kod dan aplikasi yang tidak ditandatangani secara rasmi oleh Sony.

Antara perkara yang boleh dilakukan selepas jailbreak:

1. Memasang aplikasi homebrew dalam format `.pkg`.
2. Menggunakan emulator seperti RetroArch.
3. Mengurus, memindahkan dan mengubah save game.
4. Menggunakan cheat dan game patches.
5. Mengakses fail sistem menggunakan FTP dan file manager.
6. Memasang aplikasi multimedia dan utiliti tambahan.
7. Membuat backup game daripada cakera fizikal milik sendiri.

Jailbreak tidak bermaksud kita perlu membuang sistem operasi asal PS4. Sistem asal masih digunakan, cuma kita menjalankan exploit untuk membuka fungsi tambahan.

### Apa itu GoldHEN?

GoldHEN ialah Homebrew Enabler (HEN) yang dibangunkan oleh SiSTRo.

Ia merupakan antara payload paling popular dalam komuniti PS4 jailbreak.

Antara fungsi GoldHEN:

- Debug Settings — Membuka menu tambahan untuk pemasangan package.
- Homebrew Support — Membolehkan aplikasi homebrew dijalankan.
- FTP Server — Memindahkan fail antara PS4 dan PC.
- BinLoader — Memuatkan payload melalui rangkaian.
- Remote Package Install — Memasang package melalui rangkaian.
- Cheat Menu — Mengaktifkan cheat untuk game yang disokong.
- Plugin Support — Membolehkan plugin tambahan digunakan.
- FPS Counter — Memaparkan FPS semasa bermain.
- Rest Mode Support — Mengekalkan fungsi jailbreak ketika menggunakan Rest Mode, tertakluk kepada kestabilan firmware dan payload.

GoldHEN bukan jailbreak exploit itu sendiri. Ia ialah payload yang dijalankan selepas exploit berjaya.

## Firmware Compatibility

Tidak semua firmware PS4 boleh dijailbreak menggunakan kaedah yang sama. Setiap firmware memerlukan exploit dan kernel patch yang sepadan.

Berikut ialah status yang dapat disemak pada 2 November 2026:

| Firmware | Status |
|---|---|
| 5.05 | Supported |
| 6.72 | Supported |
| 7.02 – 7.55 | Supported |
| 9.00 | Supported |
| 9.60 | Supported |
| 10.00 / 10.01 | Supported |
| 10.50 | Supported |
| 10.70 / 10.71 | Supported |
| 11.00 | Supported |
| 12.00 – 12.70 | Bergantung pada exploit dan payload |
| 13.02 | Supported |
| 13.04 | Supported |
| 13.50 | Supported |
| 13.52 | Supported |
| 14.00 | Not available |

Nota penting:

- Firmware 13.50 kini mempunyai browser-based jailbreak chain yang telah dilaporkan berjaya diuji pada hardware sebenar.
- Untuk 13.50, terdapat kernel patch dan PS4 HEN yang dilaporkan berfungsi. Jangan anggap semua versi GoldHEN serasi dengannya.
- GoldHEN v2.4b18 yang diterbitkan secara rasmi menyenaraikan sokongan sehingga firmware 11.00. Sokongan firmware lebih baharu memerlukan build atau payload yang benar-benar sepadan.
- Firmware 13.52 juga mempunyai laporan ujian payload yang berbeza. Jangan gunakan payload 13.52 pada 13.50.

Rujukan:
- https://henjb.com/
- https://github.com/caziques/PS4
- https://github.com/GoldHEN/GoldHEN/releases

## Prasyarat

Berikut ialah barang-barang dan persediaan yang diperlukan:

1. PlayStation 4

   PS4 Fat, Slim atau Pro. Firmware yang digunakan dalam panduan ini ialah 13.50.

2. Sambungan Internet

   Diperlukan untuk mengakses exploit host, memuat turun homebrew dan mendapatkan fail tambahan.

3. USB Thumb Drive

   Disarankan menggunakan USB 3.0 dengan kapasiti sekurang-kurangnya 16GB. Format kepada exFAT supaya boleh menyimpan fail berukuran lebih 4GB.

4. PC atau Laptop

   Berguna untuk memuat turun aplikasi, mengurus fail PKG dan membuat backup.

5. Ruang storan

   Sediakan ruang kosong pada internal HDD PS4 untuk memasang aplikasi dan game.

6. PS4 Internet Browser

   Browser terbina dalam PS4 akan digunakan untuk mengakses jailbreak host.

**Peringatan:** Jangan kemas kini firmware PS4 secara sembarangan. Firmware yang lebih baharu mungkin belum mempunyai exploit atau payload yang serasi. Matikan automatic system software download dan elakkan pemasangan system update secara tidak sengaja.

## Mula Jailbreak

Untuk firmware 13.50, terdapat browser-based jailbreak chain yang boleh digunakan melalui host seperti HenJB.

### Konfigurasi Network dan Custom DNS

Sebelum memulakan jailbreak, aku cadangkan supaya kita konfigurasi network PS4 menggunakan Custom DNS.

Kenapa perlu menggunakan DNS custom?

Ada dua tujuan utama:

1. Menghalang sambungan kepada server Sony yang digunakan untuk mengedarkan firmware dan game updates.
2. Memudahkan akses kepada jailbreak host melalui PS4 User Guide, bergantung kepada DNS server yang digunakan.

Ini penting kerana kita tidak mahu PS4 memuat turun firmware terkini secara automatik. Jika firmware ter-update, kemungkinan besar exploit yang digunakan tidak lagi berfungsi.

#### Cara menetapkan Custom DNS

1. Pergi ke Settings.
2. Pilih Network.
3. Pilih Set Up Internet Connection.
4. Pilih sama ada Use Wi-Fi atau Use a LAN Cable.
5. Pilih Custom.
6. Pada IP Address Settings, pilih Automatic.
7. Pada DHCP Host Name, pilih Do Not Specify.
8. Pada DNS Settings, pilih Manual.
9. Masukkan DNS berikut:

   Primary DNS: `62.210.38.117`

   Secondary DNS: Biarkan kosong jika sistem membenarkan.

   Alternatif DNS yang boleh digunakan:

   Primary DNS: `165.227.83.145`

   Secondary DNS: `192.241.221.79`

10. Pada MTU Settings, pilih Automatic.
11. Pada Proxy Server, pilih Do Not Use.
12. Pilih Test Internet Connection.

DNS `62.210.38.117` dikenali sebagai Nomadic DNS. Ia menyekat sambungan ke server Sony tertentu yang digunakan untuk firmware dan game updates, tetapi masih membenarkan akses Internet biasa.

DNS alternatif Al-Azif pula merupakan pilihan lama yang digunakan dalam komuniti PS4 jailbreak. Sesetengah fungsi Internet biasa mungkin tidak berfungsi dengan baik menggunakan DNS tersebut.

#### Penting untuk pengguna firmware 13.50

Untuk firmware 13.50, jailbreak browser-based seperti HenJB tidak semestinya memerlukan DNS custom untuk berfungsi. Kita masih boleh membuka jailbreak host secara manual melalui Internet Browser.

Jika menggunakan DNS yang mengalihkan PS4 User Guide kepada jailbreak host, kita boleh mengaksesnya melalui:

Settings > User's Guide/Helpful Info > User Guide

Jika halaman jailbreak tidak muncul, buka Internet Browser dan masukkan alamat host secara manual.

Perlu diingat bahawa DNS yang menyekat server Sony juga boleh menyebabkan sesetengah fungsi homebrew yang memerlukan sambungan kepada server PlayStation tidak berfungsi, termasuk fungsi muat turun retail updates dalam aplikasi tertentu.

Untuk perlindungan tambahan, matikan juga Automatic Downloads:

1. Pergi ke Settings.
2. Pilih System.
3. Pilih Automatic Downloads.
4. Nyahaktifkan System Software Update Files.
5. Nyahaktifkan Install Automatically jika pilihan tersebut tersedia.

Aku juga cadangkan supaya *Stay Connected to the Internet* dimatikan di *Settings > Power Save Settings > Set Functions Available in Rest Mode* sebelum jailbreak diaktifkan.

Rujukan:
- https://consolemods.org/wiki/PS4_getting_started
- https://consolemods.org/wiki/PS4:Blocking_OFW_Updates
- https://henjb.com/

### Kaedah 1: Browser-based Jailbreak

1. Hidupkan PS4 seperti biasa.
2. Pastikan firmware berada pada 13.50 melalui Settings > System > System Information.
3. Buka Internet Browser pada PS4.
4. Masukkan alamat berikut:

   `https://henjb.com/`

5. Pilih PS4.
6. Pilih firmware 13.50 jika pilihan firmware dipaparkan.
7. Ikut arahan pada halaman tersebut untuk memulakan jailbreak.
8. Tunggu sehingga proses exploit dan kernel patch selesai.
9. Jika berjaya, payload HEN yang serasi akan dimuatkan.
10. Semak sama ada menu atau fungsi homebrew telah tersedia.

Proses ini mungkin mengambil beberapa percubaan. Browser boleh menjadi tidak responsif, exploit boleh gagal dan PS4 mungkin mengalami kernel panic.

Jika berlaku kernel panic, matikan dan hidupkan semula konsol sebelum mencuba sekali lagi.

**Perhatian khusus untuk firmware 13.50:** Halaman HenJB menyenaraikan firmware ini sebagai supported, tetapi jangan terus menganggap GoldHEN rasmi v2.4b18 boleh digunakan. Pastikan payload yang ditawarkan benar-benar dibina untuk firmware 13.50. Jika hanya PS4 HEN tersedia, gunakan payload tersebut dan jangan paksa pemasangan GoldHEN daripada firmware lain.

### Kaedah 2: PPPwn

PPPwn ialah exploit yang dibangunkan oleh TheFlow. Ia menggunakan kelemahan dalam sistem pemprosesan rangkaian PPPoE pada PS4.

Kaedah ini popular untuk firmware lama, khususnya firmware 9.00 hingga 11.00.

Secara umum, ia memerlukan:

1. PC atau Raspberry Pi.
2. Kabel Ethernet.
3. Sambungan Ethernet antara PC dan PS4.
4. PPPwn loader yang sepadan dengan firmware.
5. Payload yang serasi.

Untuk firmware 13.50, gunakan browser-based chain yang telah disahkan untuk firmware tersebut dan bukannya menganggap PPPwn versi lama boleh digunakan.

<img src="https://i.imgur.com/DgwiaRe.jpg" width="60%">

## Memasang Homebrew Store

Homebrew Store ialah antara aplikasi pertama yang aku cadangkan untuk dipasang selepas berjaya jailbreak.

Ia berfungsi seperti Google Play Store, tetapi khusus untuk aplikasi homebrew PS4.

Antara kandungan yang boleh didapati:

- File Manager.
- Emulator.
- Media Player.
- Save Game Manager.
- Game Manager.
- Homebrew Games.
- Cheats Manager.
- Pelbagai aplikasi tambahan.

### Cara pemasangan

1. Jalankan jailbreak dan aktifkan HEN yang serasi.
2. Buka Internet Browser PS4.
3. Masukkan alamat berikut:

   `https://pkg-zone.com/install`

4. Tunggu proses muat turun dan pemasangan selesai.
5. Kembali ke Home Screen.
6. Cari aplikasi Homebrew Store.
7. Buka aplikasi tersebut.

Pastikan sambungan Internet aktif.

Homebrew Store akan memudahkan kita mencari, memuat turun dan mengemas kini aplikasi tanpa perlu menggunakan PC setiap kali.

## Memasang Game dalam Format PKG / FPKG

PS4 menggunakan format package `.pkg` untuk pemasangan game, update, DLC dan aplikasi.

Terdapat dua jenis package yang biasa dijumpai dalam komuniti:

1. PKG — Package rasmi yang mempunyai sistem pengesahan lesen Sony.
2. FPKG — Fake PKG, iaitu package yang telah dibina menggunakan proses tertentu supaya boleh dipasang melalui persekitaran debug atau homebrew.

FPKG sering digunakan dalam komuniti untuk memasang homebrew dan backup game yang dibuat daripada salinan permainan milik sendiri.

### Cara memasang package melalui USB

1. Sediakan USB Thumb Drive.
2. Format USB kepada exFAT.
3. Salin fail `.pkg` ke dalam root USB.
4. Sambungkan USB kepada PS4.
5. Aktifkan jailbreak dan HEN yang serasi.
6. Buka GoldHEN Debug Settings atau menu Debug Settings yang disediakan oleh HEN.
7. Pilih Package Installer.
8. Pastikan Package Source ditetapkan kepada USB.
9. Pilih fail package yang ingin dipasang.
10. Tunggu sehingga pemasangan selesai.

Jika package tersebut ialah game, ikon game akan muncul pada Home Screen.

### Susunan pemasangan game

Sebelum memasang game dan update, ada tiga perkara penting yang perlu kita faham, iaitu Title ID, Region dan Game Version.

**1. Title ID**

Title ID ialah kod unik yang diberikan kepada setiap keluaran game PS4. Biasanya ia bermula dengan `CUSA`, `CUSA` diikuti lima digit, atau kod lain seperti `PLAS`.

Contohnya:

`CUSA33778` — Star Wars Jedi: Survivor (PS4)

Title ID ini penting kerana base game, update dan DLC perlu mempunyai Title ID yang sepadan. Update untuk CUSA33778 tidak boleh digunakan pada game yang mempunyai Title ID berlainan.

**2. Region**

Region merujuk kepada kawasan atau pasaran bagi sesuatu keluaran game.

Antara region yang biasa dijumpai:

- USA / US — Amerika Syarikat.
- EUR — Eropah.
- ASIA — Asia.
- JPN — Jepun.

Walaupun nama game sama, keluaran daripada region berlainan mungkin mempunyai Title ID yang berbeza. Oleh itu, pastikan update yang dimuat turun adalah untuk keluaran game yang sama.

**3. Game Version**

Game Version menunjukkan versi game yang sedang digunakan.

Contohnya:

- v1.00 — Versi asal atau Base Game.
- v1.01 — Update pertama.
- v1.03 — Update yang mengandungi pembaikan dan penambahbaikan daripada versi sebelumnya.

Pastikan update yang dipasang serasi dengan Title ID base game. Sebaiknya gunakan update terkini yang tersedia untuk keluaran tersebut.

#### Contoh: Star Wars Jedi: Survivor (CUSA33778)

Sebagai contoh, kita gunakan game Star Wars Jedi: Survivor versi PS4.

Maklumat package yang dikenal pasti:

| Komponen | Maklumat |
|---|---|
| Game | Star Wars Jedi: Survivor |
| Title ID | CUSA33778 |
| Region | USA |
| Base Game | v1.00 |
| Game Update terkini | v1.03 |
| Saiz Base Game | Sekitar 51–54GB |
| Saiz Update v1.03 | Sekitar 122MB |

Susunan pemasangannya adalah seperti berikut:

1. Base Game — CUSA33778 v1.00.
2. Update — CUSA33778 v1.03.

Update v1.03 merupakan patch PS4 yang memperbaiki kestabilan permainan dan audio dalam cinematic.

Untuk pengguna firmware lama, terdapat juga package update yang telah dibuat backport:

- v1.03 Backport 5.05.
- v1.03 Backport 6.72–9.00.
- v1.03 Optional Fix untuk firmware 11.00.

Perbezaan backport ini ialah keperluan minimum firmware untuk menjalankan package tersebut. Oleh sebab firmware PS4 yang digunakan dalam panduan ini ialah 13.50, package biasa yang memerlukan firmware 11.00 sepatutnya berada dalam lingkungan firmware yang lebih rendah daripada 13.50. Namun, tetap pastikan package tersebut serasi dengan jailbreak dan payload yang digunakan.

Selepas pemasangan, semak Game Information untuk memastikan versi game telah dikemas kini kepada v1.03.

Nota: Jangan keliru antara firmware requirement dengan Game Version. Firmware requirement merujuk kepada versi sistem PS4 minimum yang diperlukan untuk menjalankan package, manakala Game Version merujuk kepada versi kemas kini permainan itu sendiri.

Rujukan:
- https://serialstation.com/titles/CUSA/33778
- https://www.ea.com/games/starwars/jedi-survivor/news/patch-notes

### Memasang package melalui rangkaian

Selain USB, kita juga boleh menggunakan Remote Package Installer atau Direct Package Installer.

Kaedah ini membolehkan fail dihantar daripada PC atau telefon ke PS4 melalui rangkaian tempatan.

Secara umum:

1. Aktifkan BinLoader atau fungsi Remote Package Install yang diperlukan.
2. Pastikan PC dan PS4 berada dalam rangkaian yang sama.
3. Buka aplikasi package installer yang serasi.
4. Masukkan IP Address PS4.
5. Pilih fail `.pkg`.
6. Mulakan proses pemasangan.

Kaedah ini lebih mudah apabila saiz package besar dan kita tidak mahu memindahkan fail menggunakan USB.

## Aplikasi dan Utility Berguna

Berikut ialah beberapa aplikasi yang menarik untuk dipasang.

### 1. Apollo Save Tool

Apollo Save Tool ialah aplikasi homebrew untuk mengurus save game PS4.

Antara fungsinya:

- Backup save game.
- Restore save game.
- Download save game daripada database komuniti.
- Decrypt dan encrypt save game.
- Resign save game untuk profil pengguna.
- Patch save game bagi game yang disokong.
- Mengurus sesetengah data trophy.

Ini antara aplikasi yang sangat berguna sekiranya kita mahu memindahkan save game antara dua profil atau membuat backup sebelum mencuba cheat.

**Cara menggunakan Apollo Save Tool:**

1. Muat turun Apollo Save Tool melalui Homebrew Store atau repositori rasmi.
2. Pasang fail `.pkg`.
3. Buka Apollo Save Tool dari Home Screen.
4. Pilih HDD Saves untuk melihat save game yang tersimpan dalam konsol.
5. Pilih game yang ingin diuruskan.
6. Pilih fungsi yang diperlukan.

Antara fungsi yang biasa digunakan:

- Copy Save Game — Menyalin save game ke USB.
- Export Decrypted Save Files — Mengeksport save dalam format decrypted.
- Import Decrypted Save Files — Mengimport save yang telah diedit.
- Apply Changes and Resign — Mengaplikasikan perubahan dan resign save.
- View Save Details — Melihat maklumat save game.

Apollo juga mempunyai Online Database yang membolehkan kita mendapatkan save game daripada komuniti.

Satu perkara penting: Save game PS4 mempunyai kaitan dengan User ID dan Account ID. Jangan anggap semua save game boleh terus digunakan pada semua profil.

Sebaiknya buat backup terlebih dahulu sebelum melakukan sebarang perubahan.

Repositori:
https://github.com/bucanero/apollo-ps4

### 2. Homebrew Store

Seperti yang diterangkan sebelum ini, Homebrew Store merupakan pusat pemasangan aplikasi.

Aku cadangkan gunakan aplikasi ini sebagai tempat utama untuk mendapatkan homebrew berbanding memuat turun fail `.pkg` daripada sumber yang tidak diketahui.

Antara aplikasi menarik yang boleh dicari di dalamnya:

- PS4-Xplorer.
- Itemzflow.
- RetroArch.
- Apollo Save Tool.
- Cheats Manager.

### 3. Cheats Manager

Cheats Manager membolehkan kita mendapatkan dan mengurus cheat untuk game PS4.

Contohnya:

- Infinite Health.
- Infinite Money.
- Unlimited Ammo.
- Max Level.
- Unlock Items.
- Game Speed Modifier.

Namun begitu, cheat bukan universal. Ia bergantung kepada Title ID, region dan versi update game.

**Cara penggunaan Cheats Manager:**

1. Buka Homebrew Store.
2. Cari Cheats Manager.
3. Muat turun dan pasang aplikasi tersebut.
4. Sediakan database cheat yang disokong oleh aplikasi.
5. Buka Cheats Manager dan gunakan fungsi Update untuk mengimport database.
6. Keluar daripada aplikasi.
7. Buka menu GoldHEN atau HEN yang mempunyai fungsi cheat.
8. Aktifkan Cheat Menu jika payload tersebut menyokongnya.
9. Jalankan game yang berkaitan.
10. Buka Cheat Menu dan pilih cheat yang ingin digunakan.

Untuk pengguna GoldHEN, terdapat juga cheat menu terbina dalam. Walau bagaimanapun, fungsi ini masih dianggap experimental.

Pastikan cheat sepadan dengan versi game yang digunakan. Cheat untuk CUSA00001 tidak semestinya berfungsi dengan PLAS00001 walaupun nama game sama.

### 4. PS4-Xplorer

PS4-Xplorer ialah file manager untuk PS4.

Fungsinya hampir sama seperti Windows File Explorer.

Antara perkara yang boleh dilakukan:

- Melihat folder dalam sistem PS4.
- Menyalin fail.
- Memadam fail.
- Memindahkan fail.
- Mengekstrak ZIP.
- Mengurus fail pada USB.
- Mengakses direktori aplikasi dan data.

Aplikasi ini sangat berguna apabila kita perlu mengurus fail secara terus daripada konsol tanpa menggunakan PC.

Namun, jangan padam fail sistem secara sembarangan kerana ia boleh menyebabkan aplikasi atau sistem PS4 gagal berfungsi.

### 5. Itemzflow

Itemzflow merupakan game manager untuk PS4.

Antara fungsi yang disediakan:

- Mengurus game yang telah dipasang.
- Melihat maklumat game.
- Mengurus game updates.
- Melancarkan game.
- Mengurus sesetengah kandungan game.
- Membantu proses pengurusan game dump.

Ia berguna untuk pengguna yang mempunyai banyak game dalam internal HDD dan external HDD.

### 6. RetroArch

RetroArch merupakan emulator frontend yang menyokong pelbagai platform klasik.

Antara sistem yang boleh diemulasikan bergantung kepada core yang digunakan:

- Nintendo Entertainment System (NES).
- Super Nintendo (SNES).
- Game Boy.
- Game Boy Advance.
- Sega Mega Drive.
- PlayStation 1.
- Arcade.

RetroArch menggunakan sistem core. Setiap core bertanggungjawab untuk mengemulasikan sistem tertentu.

Kita boleh memuat turun core yang diperlukan melalui menu Online Updater jika build yang digunakan menyokong fungsi tersebut.

### 7. Orbis Toolbox

Orbis Toolbox ialah aplikasi utiliti yang menyediakan pelbagai maklumat dan fungsi tambahan untuk PS4.

Antaranya:

- Memantau penggunaan CPU dan RAM.
- Memaparkan FPS.
- Mengakses maklumat sistem.
- Mengurus beberapa fungsi debugging.
- Menyediakan overlay semasa bermain game.

Aplikasi ini lebih sesuai untuk pengguna yang suka mengubah suai dan memantau prestasi konsol.

Perlu diingat bahawa sesetengah aplikasi seperti Orbis Toolbox, Itemzflow dan Apollo boleh menyebabkan masalah ketika memasuki atau keluar daripada Rest Mode pada konfigurasi tertentu.

### 8. FTP Server

FTP membolehkan kita memindahkan fail daripada PC ke PS4 melalui rangkaian tempatan.

Jika menggunakan GoldHEN, FTP Server biasanya menggunakan port 2121.

Cara asas:

1. Aktifkan FTP Server dalam menu payload.
2. Buka Settings > System > System Information untuk melihat IP Address PS4.
3. Buka FileZilla pada PC.
4. Masukkan IP Address PS4 sebagai Host.
5. Masukkan port 2121.
6. Sambungkan kedua-dua peranti melalui rangkaian yang sama.
7. Selepas berjaya, kita boleh melihat direktori PS4 melalui FileZilla.

FTP sangat berguna untuk memindahkan fail berukuran besar tanpa perlu mencabut USB berulang kali.

Jangan biarkan FTP Server terbuka pada rangkaian awam.

## Apendiks

### Mengelakkan System Update

Ini antara perkara paling penting untuk pengguna PS4 jailbreak.

Sony masih mengeluarkan firmware baharu dan setiap kemas kini berpotensi menutup kelemahan yang digunakan oleh exploit.

Untuk mengurangkan risiko:

1. Pergi ke Settings.
2. Pilih System.
3. Pilih Automatic Downloads.
4. Matikan pilihan System Software Update Files.
5. Elakkan menekan butang Update secara tidak sengaja.

Jika menggunakan payload yang mempunyai fungsi firmware update blocker, aktifkan fungsi tersebut selepas memastikan ia serasi dengan firmware.

### Backup Save Game

Sebelum menggunakan cheat, mod atau aplikasi yang mengubah data game, buat backup menggunakan Apollo Save Tool.

Sekiranya save game rosak, kita masih mempunyai salinan asal untuk dipulihkan.

### Kernel Panic dan Restart

Jailbreak PS4 tidak semestinya berjaya setiap kali.

Ada kalanya browser akan freeze, exploit gagal atau konsol mengalami kernel panic.

Ini perkara yang boleh berlaku dalam proses eksploitasi kernel.

Jika berlaku:

1. Tunggu sebentar untuk memastikan konsol benar-benar tidak responsif.
2. Matikan konsol jika perlu.
3. Hidupkan semula PS4.
4. Cuba jailbreak sekali lagi.

Jangan anggap setiap kernel panic bermaksud konsol telah brick.

### Rest Mode

Sesetengah konfigurasi jailbreak menyokong Rest Mode, tetapi kestabilannya bergantung kepada firmware, exploit dan payload.

Jangan terus menganggap jailbreak akan kekal aktif selepas setiap kali konsol memasuki Rest Mode.

Jika berlaku masalah, lakukan cold boot dan jailbreak semula.

### Port Download Game

Jangan heboh sangat dekat orang nanti port port ni hilang.

1. https://archive.org/download/ps4-fpkg-collection-english-s (tukar huruf akhir ke huruf awal game) -- dd
2. https://dlpsgame.com/ -- external link
3. https://t.me/blackbox_rush
4. https://t.me/PSPulse_Discuss -- group chat
5. https://t.me/PlayStation_Pulse
6. https://t.me/LinkRush_Bot -- external link

### Sumber dan Rujukan

1. HenJB — https://henjb.com/
2. GoldHEN — https://github.com/GoldHEN/GoldHEN
3. PS4 Exploit Research — https://github.com/caziques/PS4
4. Apollo Save Tool — https://github.com/bucanero/apollo-ps4
5. ConsoleMods Wiki — https://consolemods.org/wiki/PS4
6. Homebrew Store — https://pkg-zone.com/