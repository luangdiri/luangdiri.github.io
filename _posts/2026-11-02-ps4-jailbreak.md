---
title: "PS4 Jailbreak (GoldHEN, Homebrew & FPKG)"
date: 2026-11-02 15:39:50
lastmod: 2026-11-18 01:20:19
tags:
- tutorial
---

<img src="https://i.imgur.com/DLbTNUV.png">

Di sebabkan PS4 aku tak berusik sampai naik habuk, lebih baik aku jailbreak je benda ni dari tak buat apa-apa. Kalau zaman PS2 dulu kita menggunakan [Free McBoot (FMCB)](https://luangdiri.github.io/2020/05/31/ps2-free-mcboot-fmcb.html), untuk PS4 pula mempunyai beberapa kaedah jailbreak seperti WebKit Exploit, PPPwn dan GoldHEN.

Jailbreak PS4 ini membuka banyak kemungkinan. Antaranya memasang aplikasi homebrew, menggunakan emulator, mengurus save game, memasang mod dan cheat, menggunakan file manager serta menjalankan pelbagai utiliti yang tidak disediakan oleh Sony.

---

**Kandungan:**

- [Apa itu PS4 Jailbreak?](#apa-itu-ps4-jailbreak)
  - [Apa itu GoldHEN?](#apa-itu-goldhen)
- [Firmware Compatibility](#firmware-compatibility)
- [Mula Jailbreak](#mula-jailbreak)
  - [Konfigurasi Network dan Custom DNS](#konfigurasi-network-dan-custom-dns)
    - [Cara menetapkan Custom DNS](#cara-menetapkan-custom-dns)
    - [Sebelum melakukan jailbreak](#sebelum-melakukan-jailbreak)
  - [Kaedah 1: Browser-based Jailbreak](#kaedah-1-browser-based-jailbreak)
  - [Kaedah 2: PPPwn](#kaedah-2-pppwn)
- [Memasang Homebrew Store](#memasang-homebrew-store)
  - [Cara pemasangan](#cara-pemasangan)
- [Memasang Game dalam Format PKG / FPKG](#memasang-game-dalam-format-pkg--fpkg)
  - [Cara memasang package melalui USB](#cara-memasang-package-melalui-usb)
  - [Susunan pemasangan game](#susunan-pemasangan-game)
    - [Contoh: Star Wars Jedi: Survivor (CUSA33778)](#contoh-star-wars-jedi-survivor-cusa33778)
  - [Memasang package melalui rangkaian](#memasang-package-melalui-rangkaian)
- [Apendiks](#apendiks)
  - [Aplikasi dan Utility Berguna](#aplikasi-dan-utility-berguna)
  - [Mengelakkan System Update](#mengelakkan-system-update)
  - [Backup Save Game](#backup-save-game)
  - [Kernel Panic dan Restart](#kernel-panic-dan-restart)
  - [Rest Mode](#rest-mode)
  - [Port Download Game](#port-download-game)
- [Sumber dan Rujukan](#sumber-dan-rujukan)

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

GoldHEN ialah **Homebrew Enabler (HEN)** yang dibangunkan oleh [SiSTRo](https://github.com/SiSTR0).

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

Tidak semua firmware PS4 boleh dijailbreak menggunakan kaedah yang sama. Setiap firmware memerlukan exploit dan kernel patch yang sepadan. Mujur lah version firmware PS4 aku boleh support jailbreak ni.

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

Rujukan:
- https://henjb.com/
- https://github.com/caziques/PS4
- https://github.com/GoldHEN/GoldHEN/releases

## Mula Jailbreak

Untuk firmware 13.50 yang aku guna, terdapat browser-based jailbreak chain yang boleh digunakan melalui host seperti HenJB.

### Konfigurasi Network dan Custom DNS

Sebelum memulakan jailbreak, aku cadangkan supaya kita konfigurasi network PS4 menggunakan *Custom DNS*.

Kenapa perlu menggunakan DNS custom?

Ada dua tujuan utama:

1. Menghalang sambungan kepada server Sony yang digunakan untuk mengedarkan firmware dan game updates.
2. Memudahkan akses kepada jailbreak host melalui PS4 User Guide, bergantung kepada DNS server yang digunakan.

Ini penting kerana kita tidak mahu PS4 memuat turun firmware terkini secara automatik. Jika firmware ter-update, kemungkinan besar exploit yang digunakan tidak lagi berfungsi.

#### Cara menetapkan Custom DNS

1. Pergi ke **Settings**.
2. Pilih **Network**.
3. Pilih **Set Up Internet Connection**.
4. Pilih sama ada Use **Wi-Fi** atau **Use a LAN Cable**.
5. Pilih **Custom**.
6. Pada **IP Address Settings**, pilih **Automatic**.
7. Pada **DHCP Host Name**, pilih **Do Not Specify**.
8. Pada **DNS Settings**, pilih **Manual**.
9. Masukkan DNS berikut:

   **Primary DNS**: `62.210.38.117`  
   **Secondary DNS**: `45.56.67.85` (atau biarkan kosong)

10. Pada **MTU Settings**, pilih **Automatic**.
11. Pada **Proxy Server**, pilih **Do Not Use**.
12. Pilih **Test Internet Connection**.

DNS ini dikenali sebagai Nomadic DNS. Ia menyekat sambungan ke server Sony tertentu yang digunakan untuk firmware dan game updates, tetapi masih membenarkan akses Internet biasa.

#### Sebelum melakukan jailbreak

Untuk perlindungan tambahan, matikan juga **Automatic Downloads**:

1. Pergi ke **Settings**.
2. Pilih **System**.
3. Pilih **Automatic Downloads**.
4. Nyahaktifkan **System Software Update Files**.
5. Nyahaktifkan **Install Automatically** jika pilihan tersebut tersedia.

<img src="https://i.imgur.com/ugTei87.jpg" width="80%">

Aku juga cadangkan supaya *Stay Connected to the Internet* dimatikan di *Settings > Power Save Settings > Set Functions Available in Rest Mode* sebelum jailbreak diaktifkan.

Rujukan:
- https://consolemods.org/wiki/PS4_getting_started
- https://consolemods.org/wiki/PS4:Blocking_OFW_Updates
- https://henjb.com/

### Kaedah 1: Browser-based Jailbreak

1. Hidupkan PS4 seperti biasa.
2. Lihat versi firmware melalui *Settings > System > System Information*.
3. Pastikan versi nya supported berdasarkan table di [atas](#firmware-compatibility).
4. Buka Internet Browser pada PS4.
5. Masukkan alamat berikut (bookmark URL ni):

   `https://henjb.com/`

6. Pilih PS4.
7. Pilih firmware PS4 jika pilihan firmware dipaparkan.
8. Ikut arahan pada halaman tersebut untuk memulakan jailbreak.
9. Tunggu sehingga proses exploit dan kernel patch selesai.
10. Jika berjaya, payload HEN yang serasi akan dimuatkan.
11. Semak sama ada menu atau fungsi homebrew telah tersedia.

**Nota:** Lepas dah jailbreak untuk kali pertama, exploit ni akan aktif selama kau turn on konsol kau. Bila kau turn off, ia automatik akan terpadam dan kena mulakan langkah ke-4 balik (buka Internet Browser etc.). Jangan risau, payload exploit tu dah di simpan dalam cache browser, jadi ia juga boleh di aktifkan walaupun tiada internet. Ini bukan bug, tapi feature.

Proses ini mungkin mengambil beberapa percubaan. Browser boleh menjadi tidak responsif, exploit boleh gagal dan PS4 mungkin mengalami kernel panic.

Jika berlaku kernel panic, matikan dan hidupkan semula konsol sebelum mencuba sekali lagi.

**Perhatian:** Pastikan payload yang ditawarkan benar-benar dibina untuk firmware versi kau. Jika hanya PS4 HEN tersedia, gunakan payload tersebut dan jangan paksa pemasangan GoldHEN daripada firmware lain.

Lepas aku buat jailbreak guna cara ni, terus rasa teruja sebab ini first time aku dapat saksikan "hack" yang tak perlu step yang banyak macam zaman PS2 dulu. Serius, kagum dengan hacker zaman moden sekarang.

### Kaedah 2: PPPwn

Yang ni aku tak pernah try. Tapi untuk ilmu tambahan, aku tulis juga.

PPPwn ialah exploit yang dibangunkan oleh [TheFlow](https://github.com/TheOfficialFlow/pppwn). Ia menggunakan [kelemahan](https://hackerone.com/reports/2177925) dalam sistem pemprosesan rangkaian PPPoE pada PS4.

Kaedah ini popular untuk firmware lama, khususnya firmware 9.00 hingga 11.00.

Secara umum, ia memerlukan:

1. PC atau Raspberry Pi.
2. Kabel Ethernet.
3. Sambungan Ethernet antara PC dan PS4.
4. PPPwn loader yang sepadan dengan firmware.
5. Payload yang serasi.

Cuma sekarang dah ada browser exploit, lebih mudah dan cepat.

<img src="https://i.imgur.com/DgwiaRe.jpg" width="80%">

## Memasang Homebrew Store

Homebrew Store ialah antara aplikasi pertama yang aku cadangkan untuk dipasang selepas berjaya jailbreak.

Ia berfungsi seperti Google Play Store, tetapi khusus untuk aplikasi homebrew PS4.

Antara kandungan yang boleh didapati:

- File Manager
- Emulator
- Media Player
- Save Game Manager
- Game Manager
- Homebrew Games
- Cheats Manager

Dan macam-macam lagi.

### Cara pemasangan

Pastikan sambungan Internet aktif.

1. Jalankan jailbreak.
2. Buka Internet Browser PS4.
3. Masukkan alamat berikut:

   `https://pkg-zone.com/install`

4. Tunggu proses muat turun dan pemasangan selesai.
5. Kembali ke Home Screen.
6. Cari aplikasi Homebrew Store.
7. Buka aplikasi tersebut.

Kau boleh juga download `.pkg` file nya dari laptop, masuk ke USB dan install dari USB. Tapi ini cara alternatif.

## Memasang Game dalam Format PKG / FPKG

**Nota:** Untuk nak cari game untuk di muat turun, ada di bahagian Apendiks di [bawah](#port-download-game).

PS4 menggunakan format package `.pkg` untuk pemasangan game, update, DLC dan aplikasi.

Terdapat dua jenis package yang biasa dijumpai dalam komuniti:

1. PKG — Package rasmi yang mempunyai sistem pengesahan lesen Sony. Biasanya file yang orang rip direct dari CD game fizikal dan tak melakukan apa-apa perubahan dalam metadata game tersebut.
2. FPKG — Fake PKG, iaitu package yang telah dibina menggunakan proses tertentu supaya boleh dipasang melalui persekitaran debug atau homebrew.

Kebiasaan nya, kita akan guna FPKG untuk main game dalam PS4 yang dah di jailbreak.

### Cara memasang package melalui USB

1. Sediakan USB Thumb Drive.
2. Format USB kepada **exFAT**.
3. Salin fail `.pkg` ke dalam *root folder* USB tersebut.
4. Sambungkan USB kepada PS4.
5. Aktifkan jailbreak guna browser PS4.
6. Buka GoldHEN **Debug Settings**.
7. Pilih **Package Installer**.
8. Pastikan *Package Source* ditetapkan kepada **USB**.
9. Pilih fail package yang ingin dipasang.
10. Tunggu sehingga pemasangan selesai.
11. ???
12. Profit!!11

### Susunan pemasangan game

Sebelum memasang game dan update, ada tiga perkara penting yang perlu kita faham, iaitu **Title ID**, **Region** dan **Game Version**.

**1. Title ID**

Title ID ialah kod unik yang diberikan kepada setiap keluaran game PS4. Biasanya ia bermula dengan `CUSA`, `CUSA` diikuti lima digit, atau kod lain seperti `PLAS`.

Contohnya:

<img src="https://i.imgur.com/RwpeD2Y.jpg" width="80%">

`CUSA33778` — [Star Wars Jedi: Survivor](https://dlpsgame.com/star-wars-jedi-survivor-ps4-pkg/) (PS4)

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

Sebagai contoh, kita gunakan game *[Star Wars Jedi: Survivor](https://dlpsgame.com/star-wars-jedi-survivor-ps4-pkg/)* versi PS4.

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
3. DLC — kalau ada, dan pastikan kod Title ID nya sama.

**Pastikan install Base Game dahulu** sebelum install Update file. Kalau ada DLC pun boleh install lepas install semua Update file.

Untuk pengguna firmware lama, terdapat juga package update yang telah dibuat backport:

- v1.03 Backport 5.05.
- v1.03 Backport 6.72–9.00.
- v1.03 Optional Fix untuk firmware 11.00.

Perbezaan backport ini ialah keperluan minimum firmware untuk menjalankan package tersebut. Oleh sebab firmware PS4 yang digunakan dalam panduan ini ialah 13.50, package biasa yang memerlukan firmware 11.00 sepatutnya berada dalam lingkungan firmware yang lebih rendah daripada 13.50. Namun, tetap pastikan package tersebut serasi dengan jailbreak dan payload yang digunakan.

Selepas pemasangan, semak Game Information untuk memastikan versi game telah dikemas kini kepada v1.03.

**Nota:** Jangan keliru antara firmware requirement dengan Game Version. Firmware requirement merujuk kepada versi sistem PS4 minimum yang diperlukan untuk menjalankan package, manakala Game Version merujuk kepada versi kemas kini permainan itu sendiri.

Rujukan:
- https://serialstation.com/titles/CUSA/33778
- https://www.ea.com/games/starwars/jedi-survivor/news/patch-notes

### Memasang package melalui rangkaian

Selain USB, kita juga boleh menggunakan Remote Package Installer atau Direct Package Installer.

Kaedah ini membolehkan fail dihantar daripada PC atau telefon ke PS4 melalui rangkaian tempatan.

Secara umum:

1. Aktifkan **BinLoader** atau fungsi **Remote Package Install** yang diperlukan.
2. Pastikan PC dan PS4 berada dalam rangkaian yang sama.
3. Buka aplikasi package installer yang serasi.
4. Masukkan IP Address PS4.
5. Pilih fail `.pkg`.
6. Mulakan proses pemasangan.

Kaedah ini lebih mudah apabila saiz package besar dan kita tidak mahu memindahkan fail menggunakan USB.

## Apendiks

### Aplikasi dan Utility Berguna

Berikut ialah beberapa aplikasi homebrew yang berguna untuk PS4 jailbreak.

**1. Apollo Save Tool**

Aplikasi untuk mengurus save game PS4. Menyokong fungsi backup, restore, decrypt, resign dan import save game. Turut menyediakan akses kepada database save game komuniti.

GitHub: https://github.com/bucanero/apollo-ps4

**2. Homebrew Store**

Berfungsi seperti app store khusus untuk PS4 jailbreak. Menyediakan pelbagai aplikasi homebrew, emulator, file manager dan utility yang boleh dimuat turun terus melalui PS4.

Website: https://pkg-zone.com/

**3. Cheats Manager**

Aplikasi untuk mendapatkan dan mengurus database cheat game PS4 seperti Infinite Health, Infinite Money, Unlimited Ammo dan sebagainya. Keserasian cheat bergantung kepada Title ID dan versi game.

**4. PS4-Xplorer**

File manager yang membolehkan kita mengakses, menyalin, memindahkan, memadam dan mengekstrak fail terus daripada sistem PS4.

**5. Itemzflow**

Game manager untuk mengurus koleksi game yang telah dipasang. Menyediakan maklumat game, pengurusan kandungan dan pelbagai fungsi tambahan untuk game library.

**6. RetroArch**

Emulator frontend yang menyokong pelbagai platform retro seperti NES, SNES, Game Boy, Game Boy Advance, Sega Mega Drive dan PlayStation 1 melalui penggunaan emulator cores.

**7. Orbis Toolbox**

System utility untuk memantau prestasi PS4. Menyediakan maklumat CPU, RAM, FPS counter dan pelbagai overlay tambahan.

**8. FTP Server**

Membolehkan pemindahan fail antara PS4 dan PC melalui rangkaian tempatan. Sangat berguna untuk mengurus fail tanpa perlu menggunakan USB.

Untuk GoldHEN, FTP Server biasanya menggunakan port 2121.

### Mengelakkan System Update

Ini antara perkara paling penting untuk pengguna PS4 jailbreak.

Sony masih mengeluarkan firmware baharu dan setiap kemas kini berpotensi menutup kelemahan yang digunakan oleh exploit.

Untuk mengurangkan risiko:

1. Pergi ke **Settings**.
2. Pilih **System**.
3. Pilih **Automatic Downloads**.
4. Matikan pilihan **System Software Update Files**.
  
**Yang paling penting:** Elakkan menekan butang Update secara tidak sengaja.

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

1. https://archive.org/download/ps4-fpkg-collection-english-s (tukar huruf akhir ke huruf awal game) -- direct download tapi slow
2. https://dlpsgame.com/ -- external link
3. https://t.me/blackbox_rush
4. https://t.me/PSPulse_Discuss -- group chat
5. https://t.me/PlayStation_Pulse
6. https://t.me/LinkRush_Bot -- external link

Happy keluar penjara!

## Sumber dan Rujukan

1. HenJB — https://henjb.com/
2. GoldHEN — https://github.com/GoldHEN/GoldHEN
3. PS4 Exploit Research — https://github.com/caziques/PS4
4. Apollo Save Tool — https://github.com/bucanero/apollo-ps4
5. ConsoleMods Wiki — https://consolemods.org/wiki/PS4
6. Homebrew Store — https://pkg-zone.com/