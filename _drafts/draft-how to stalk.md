---
title: "Mengintip Orang Di Internet"
date: 2025-07-04 15:41:25
updated: 2025-07-04 15:41:22
tags:
---

**Kandungan:**

- [Image lookup](#image-lookup)
  - [EXIF](#exif)
  - [Reverse image](#reverse-image)
  - [Geo reverse](#geo-reverse)
- [Search engine](#search-engine)
  - [Normal searchd](#normal-searchd)
  - [Google Dorking](#google-dorking)
- [Username](#username)
- [Phone number](#phone-number)
- [Social engineering](#social-engineering)

---

Berikut adalah cara-cara untuk membuat penyelidikan tentang seseorang dalam internet.
Dipanggil OSINT (Open Source Intelligence) menggunakan sumber data terbuka yang dah sedia ada di internet.
Untuk tujuan research, journalism atau digital forensik.


## Image lookup

### EXIF

Exchangeable Image File Format (EXIF) adalah sebuah standard metadata yang di simpan di dalam file image seperti jpeg, png dan sebagainya. Metadata ini merangkumi pelbagai jenis informasi seperti timestamp, location, description dan sebagainya. EXIF ini amat penting untuk menyimpan data mengenai gambar tersebut terutamanya untuk fotografer yang ingin melihat kembali setting kamera nya ketika menangkap sesebuah foto sebagai contoh, aperture, shutter speed, ISO, dll.

Setiap jenis image file ada EXIF data ini. Untuk membacanya, kita cuma perlu *right-click* sebuah image file dan ketik `Properties`. Metadata gambar tersebut ada dibawah tab `Details`.

Untuk tujuan reconnaisance, kita cuma berminat untuk mengambil koordinat GPS atau apa-apa yang membawa makna kepada objektif penyeldikan kita. Kebanyakkan fon kamera sekarang telah dipasang fungsi save image location, jadi makin mudah kerja pengintipan kita.

### Reverse image

Teknik ini adalah untuk kita kenalpasti dari mana datangnya sesebuah gambar. Contoh situasi: ada awek cantik tiba-tiba chat kita di Telegram. Kita boleh save profile picture dia dan kita cuba pakai reverse image tool untuk kenalpasti gambar itu dicuri dari tempat lain atau memang asli dia. Dengan cara ini juga, kita dapat mengelakkan diri daripada di love scam.

Beberapa tools yang boleh dipakai untuk reverse image adalah:

1. Yandex Images
2. Google Images
3. Google Lens
4. Tineye

Bagaimana ia berfungsi adalah kita cuma masukkan gambar yang ingin kita tengok punca nya, dan tools tersebut akan keluarkan image-image yang seakan sama dengan gambar yang kita masukkan tadi.

### Geo reverse

1. AI

Geospy.ai - masukkan gambar, AI akan carikan lokasi yang ada di dalam gambar tersebut. Tak perlu koordinat GPS, cuma perlukan gambar yang ada objek atau bangunan yang jelas.

1. Coordinate

![exif-gps](https://i.imgur.com/8P7SIPn.png)

*Rajah: Koordinat GPS dalam EXIF*

Harus diingatkan bahawa messenger app seperti WhatsApp dan Telegram secara automatik menghapuskan EXIF data ini sekiranya ia di muatnaik secara normal image atas sebab compression. Pastikan gambar tersebut di kongsikan dalam bentuk "file" dan bukan "image".

Selain koordinat GPS, kita juga boleh dapat bermacam jenis info dalam metadata. Semua nya bergantung atas objektif penyelidikan kita. Kalau filename image tersebut mengandungi sesuatu yang sensitif seperti nama atau alamat atau mungkin ID sesebuah social media, maka itu juga boleh kita jadikan bahan untuk ditambah dalam reconnaisance kita.

Tools:
- https://www.pic2map.com/

## Search engine

### Normal searchd
      1. IC
      2. Name
      3. Address

### Google Dorking
   
   1. People search
      1. Pipl
      2. YellowPages (no more)
      4. Facebook search

## Username

1. Username checkers
2. By id
3. Tiktok
4. Instagram
5. Youtube
6. Telegram
7. Facebook

## Phone number
1. Truecaller
2. t.me
3. wa.me
4. Import by contacts

## Social engineering
   1. Friending with mutuals
