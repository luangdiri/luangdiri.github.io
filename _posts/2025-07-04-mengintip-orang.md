---
title: "Mengintip Orang"
date: 2025-07-04 15:41:25
updated: 2025-07-19 08:37:03
tags:
- other
---

**Kandungan:**

- [Pencarian imej](#pencarian-imej)
  - [EXIF](#exif)
  - [Reverse image](#reverse-image)
- [Reverse geolocation](#reverse-geolocation)
- [Enjin carian](#enjin-carian)
  - [Pencarian normal](#pencarian-normal)
  - [Google Dorks](#google-dorks)
  - [Pencarian orang](#pencarian-orang)
- [Username](#username)
- [Nombor telefon](#nombor-telefon)
- [Kejuruteraan sosial](#kejuruteraan-sosial)
- [Contoh](#contoh)

---

Berikut adalah cara-cara untuk membuat penyelidikan tentang seseorang dalam internet. Dipanggil Open Source Intelligence (OSINT) menggunakan sumber data terbuka yang dah sedia ada di internet. Semua cara-cara ini adalah terhad kepada pengetahuan aku saja. Mungkin banyak lagi cara. Aku post ni adalah untuk pembelajaran dan dokumentasi untuk diri sendiri. 

## Pencarian imej

### EXIF

Exchangeable Image File Format (EXIF) adalah sebuah standard metadata yang di simpan di dalam file image seperti jpeg, png dan sebagainya. Metadata ini merangkumi pelbagai jenis informasi seperti timestamp, location, description dan sebagainya. EXIF ini amat penting untuk menyimpan data mengenai gambar tersebut terutamanya untuk fotografer yang ingin melihat kembali setting kamera nya ketika menangkap sesebuah foto sebagai contoh, aperture, shutter speed, ISO, dll.

Setiap jenis image file ada EXIF data ini. Untuk membacanya, kita cuma perlu *right-click* sebuah image file dan ketik `Properties`. Metadata gambar tersebut ada dibawah tab `Details`.

Untuk tujuan reconnaisance, kita cuma berminat untuk mengambil koordinat GPS atau apa-apa yang membawa makna kepada objektif penyeldikan kita. Kebanyakkan fon kamera sekarang telah dipasang fungsi save image location, jadi makin mudah kerja pengintipan kita.

Lihat seksyen Geo reverse untuk contoh data koordinat di dalam EXIF.

### Reverse image

Teknik ini adalah untuk kita kenalpasti dari mana datangnya sesebuah gambar. Contoh situasi: ada awek cantik seksi tiba-tiba chat kita di Telegram. Kita boleh save profile picture dia dan pakai reverse image tool untuk kenalpasti gambar itu dicuri dari tempat lain atau memang asli dia. Dengan cara ini juga, kita dapat mengelakkan diri daripada di *love scam*.

Beberapa tools yang boleh dipakai untuk reverse image adalah:

1. [Yandex Images](https://yandex.com/images/)
2. [Google Images](https://images.google.com/)
3. [Google Lens](https://lens.google/) - tersedia di dalam phone Android versi terkini
4. [Tineye](https://www.tineye.com/)

Cuma masukkan gambar yang ingin kita tengok sumber nya, dan tools tersebut akan keluarkan image-image yang seakan sama dengan gambar yang kita masukkan tadi termasuk pautan dari mana gambar itu berpunca.

## Reverse geolocation

1. AI

    [Geospy.ai](https://Geospy.ai) - masukkan gambar, AI akan carikan lokasi yang ada di dalam gambar tersebut. Tak perlu koordinat GPS, cuma perlukan gambar yang ada objek atau bangunan yang jelas.

2. Koordinat (dari EXIF)

    ![exif-gps](https://i.imgur.com/8P7SIPn.png)

    *Rajah: Koordinat GPS dalam EXIF*

    Harus diingatkan bahawa messenger app seperti WhatsApp dan Telegram secara automatik menghapuskan EXIF data ini sekiranya ia di muatnaik secara normal atas sebab compression. Pastikan gambar tersebut di kongsikan dalam bentuk "file" dan bukan photo biasa. Foto HD pun tak boleh kerana EXIF nya sudah dihapus.

    Selain koordinat GPS, kita juga boleh dapat bermacam jenis info dalam metadata. Semua nya bergantung atas objektif penyelidikan kita. Kalau filename image tersebut mengandungi sesuatu yang sensitif seperti nama atau alamat atau mungkin ID sesebuah social media, maka itu juga boleh kita jadikan bahan untuk ditambah dalam reconnaisance kita.

    Tools:
    - [Pic2map.com](https://www.pic2map.com/)

## Enjin carian

### Pencarian normal

Dalam pencarian normal ini, macam biasalah kita cuma perlukan apa-apa jenis info ringkas tentang seseorang seperti nama, alamat, tempat kerja, malah tempat dia belajar pun boleh. Kebiasaannya info-info ini digunakan sebagai info tambahan untuk menambahbaik info yang telah kita ada.

### Google Dorks

Teknik dork ini dapat menghasilkan carian yang lebih tajam dan detail. Dulu aku guna dorking ini untuk cari lagu-lagu mp3 dari server yang index nya tak tertutup. Selain itu para pelajar juga boleh guna teknik dork ini untuk mencari bahan kajian seperti file pdf atau pptx. Dalam konteks penyelidikan individu pula, kita boleh pakai teknik ini untuk memperkecilkan lagi keputusan carian kita terhadap seseorang.

Berikut adalah senarai filter yang ada dalam Google dork ini bersama contohnya:

| Filter          | Description                                        | Example                              |
| :-------------- |:---------------------------------------------------| :------------------------------------|
| allintext      | Searches for occurrences of all the keywords given. | `allintext:"keyword"` |
| intext      | Searches for the occurrences of keywords all at once or one at a time. | `intext:"keyword"` |
| inurl      | Searches for a URL matching one of the keywords. | `inurl:"keyword"` |
| allinurl      | Searches for a URL matching all the keywords in the query. | `allinurl:"keyword"` |
| intitle      | Searches for occurrences of keywords in title all or one. | `intitle:"keyword"` |
| allintitle      | Searches for occurrences of keywords all at a time. | `allintitle:"keyword"` |
| site      | Specifically searches that particular site and lists all the results for that site. | `site:"www.google.com"` |
| filetype      | Searches for a particular filetype mentioned in the query. | `filetype:"pdf"` |
| link      | Searches for external links to pages. | `link:"keyword"` |
| numrange      | Used to locate specific numbers in your searches. | `numrange:321-325` |
| before/after      | Used to search within a particular date range. | `filetype:pdf & (before:2000-01-01 after:2001-01-01)` |
| allinanchor (and also inanchor)      | This shows sites which have the keyterms in links pointing to them, in order of the most links. | `inanchor:rat` |
| allinpostauthor (and also inpostauthor)      | Exclusive to blog search, this one picks out blog posts that are written by specific individuals. | `allinpostauthor:"keyword"` |
| related      | List web pages that are “similar” to a specified web page. | `related:www.google.com` |
| cache      | Shows the version of the web page that Google has in its cache. | `cache:www.google.com` |

[Sumber](https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06)
   
### Pencarian orang

Berbeza dengan pencarian normal, dalam mencari orang secara spesifik nya, kita ada beberapa tools dan website yang boleh membuat carian yang lebih mendalam tentang seseorang. Maksud nya di sini, enjin carian ini boleh mengagregatkan keputusan carian di dalam satu halaman, sebagai contoh, sosial media seseorang, no telefon yang terkait dengan individu itu, rekod awam seperti keputusan peperiksaan sekolah, kerajaan atau komersial.

Tak banyak dah website yang memberikan info ini secara percuma. Zaman MySpace dulu memang senang sebab company company ini tak mementingkan duit.

1. Pipl

    Pernah suatu ketika dulu diberi perkhidmatan percuma, tapi sekarang dikenakan bayaran dan digunakan untuk tujuan korporat dan penguatkasaan undang-undang.

2. YellowPages

    Buku tebal warna kuning, suatu ketika dulu digunakan untuk mencari no telefon berserta nama pemilik nombor tersebut. Sekarang dah ada versi online.

3. Lullar

    Enjin pencarian berdasarkan email

4. Social-Searcher.com

    Carian public mention di sosial media

5. Facebook search

    Kadang-kadang orang gunakan nama sebenar dalam Facebook, jadi Facebook masih berguna untuk mencari orang.

6. LinkedIn

    Website paling banyak info tentang pekerjaan seseorang. Kebiasaannya pengguna di sini memakai nama seperti dalam IC atas tujuan mencari kerja atau menunjuk pangkat (ego boosting).

## Username

Guna tool seperti [Instant Username](https://instantusername.com/) untuk mencari username yang telah terpakai di dalam mana-mana sosial media dan website. Dengan cara ini, kita boleh langsung melihat semua sosial media yang telah didaftarkan dengan username yang kita dapat.

Nota: Lihat [OSINT Framework](https://osintframework.com/) untuk senarai OSINT tools tambahan.

## Nombor telefon

Jika ada no telefon seseorang waima dapat panggilan atau SMS yang tidak dikenali, kau boleh langsung cek pengguna nombor tersebut melalui beberapa cara:

1. TrueCaller atau aplikasi scam call

    Aplikasi anti-scam. Tinggal masukkan saja nombor ke dalam app ini, dan database mereka akan carikan (jika ada) rekod nama pengguna nombor tersebut.

2. Shortlink
   * **WhatsApp** - `wa.me`

      Guna shortlink `https://wa.me/` diikuti nombor berserta kod negara di belakang link. Contoh `https://wa.me/+60122350988`. Klik pautan ini dan jika nombor ini telah berdaftar dalam WhatsApp, ia akan membawa terus ke chat di WhatsApp.

   * **Telegram** - `t.me`

      Sama seperti WhatsApp, tulis nombor di belakang, contoh `https://t.me/+60122350988` dan ia akan bawa masuk ke dalam app Telegram jika nombor tersebut telah didaftar bersama Telegram.

3. Import by contacts

    Ada beberapa platform yang menyokong import by contacts seperti Instagram, Facebook dan Twitter. Dengan cara meng-import contact list, platform tersebut boleh menunjukkan pada siapa nombor berkenaan telah didaftarkan. Cuma zaman sekarang, platform sosial media tidak lagi menunjukkan secara langsung nombor siapa didaftar oleh siapa, cuma dia akan tunjukkan "suggested user". Dari situ kita boleh berbudi bicara mengenalpasti siapakah tuan punya nombor.

## Kejuruteraan sosial

> *Individu A nak stalk individu B, tapi individu B merahsiakan keberadaan dia. Seorang jurutera sosial boleh kenal menjejak individu B dengan berkenalan dengan individu C, iaitu sahabat baik individu B.*

Itu lah social engineering: teknik eksploitasi menggunakan kelemahan manusia. Teknik hacking yang paling berkesan sampai ke abad ini kerana ia melibatkan emosi dan psikologi manusia yang rapuh jika nak dibandingkan dengan eksploitasi mesin.

Ikut kreativiti masing-masing, banyak lagi metode social engineering yang wujud dalam dunia ni. Kalau nak tahu, kebanyakkan kebocoran data yang berlaku selama ini adalah dari kesilapan manusia sendiri.

## Contoh

Basically, rutin aku kalau dapat nombor yang tak kenal tiba-tiba call.

1. Check melalui **TrueCaller**
2. Check pakai `wa.me` shortlink untuk check profil WhatsApp nombor tersebut
3. Check pakai `t.me` shortlink untuk check profil Telegram nombor tersebut
4. Kalau dapat info seperti nama, gambar profil atau sebagainya:
   - Google search
   - Reverse image
   - Facebook search
   - Instagram search

Banyak lagi senario yang kita hadapi selain orang yang tak kenal call kita. Mungkin jumpa tengah-tengah jalan, atau tiba-tiba dapat surat layang dari seseorang. Gunakan internet sebagai titik permulaaan anda menjadi detektif.