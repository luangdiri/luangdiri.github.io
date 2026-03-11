---
title: 'Lindungi Keluarga Anda'
date: 2026-03-12 00:35:10
updated_at: 2026-03-12 00:35:15
tags:
- tutorial
---

Internet memberi banyak manfaat kepada keluarga, tetapi ia juga membawa beberapa risiko. Tanpa sistem penapisan yang sesuai, pengguna boleh terdedah kepada pelbagai jenis ancaman.

Ancaman ini bukan hanya berkaitan kandungan dewasa. Banyak laman web juga mengandungi iklan berbahaya, skrip penjejakan, dan malware yang boleh mencuri data peribadi. Serangan seperti phishing, malicious ads (malvertising), dan laman web palsu boleh menyebabkan kebocoran maklumat, akaun digodam, atau peranti dijangkiti malware.

Sistem penapisan internet membantu mengurangkan risiko ini dengan menyekat:

* laman web berbahaya
* kandungan dewasa
* iklan yang mengandungi malware
* tracker yang mengumpul data pengguna

Pendekatan yang lebih berkesan ialah menggunakan beberapa lapisan penapisan serentak, seperti DNS filtering, hosts file, tetapan browser, dan extension.

---

**Kandungan:**  
- [Family DNS](#family-dns)
  - [Kepentingan IPv6 dalam DNS](#kepentingan-ipv6-dalam-dns)
  - [Tips konfigurasi](#tips-konfigurasi)
- [Hosts File](#hosts-file)
  - [Cara edit hosts file (Windows)](#cara-edit-hosts-file-windows)
- [Guna Brave Browser](#guna-brave-browser)
  - [Secure DNS](#secure-dns)
  - [Enable Content Filtering](#enable-content-filtering)
- [Browser Extension](#browser-extension)
- [SafeSearch Enforcement](#safesearch-enforcement)
- [Router-Level Blocking](#router-level-blocking)
- [Block DNS Over HTTPS (DoH)](#block-dns-over-https-doh)
- [Device-Level Parental Control](#device-level-parental-control)
- [Gunakan Akaun Tanpa Admin](#gunakan-akaun-tanpa-admin)
- [Kesimpulan](#kesimpulan)

## Family DNS

Family DNS ialah servis DNS yang sudah siap dengan penapisan kandungan seperti malware, iklan berbahaya, dan laman dewasa.

| DNS                           | IPv4                                   | IPv6                                             | Description                                               |
| ----------------------------- | -------------------------------------- | ------------------------------------------------ | --------------------------------------------------------- |
| Cloudflare for Families       | `1.1.1.3`<br>`1.0.0.3`                 | `2606:4700:4700::1113`<br>`2606:4700:4700::1003` | Menyekat malware dan kandungan dewasa                     |
| OpenDNS FamilyShield          | `208.67.222.123`<br>`208.67.220.123`   | `2620:0:ccc::2`<br>`2620:0:ccd::2`               | Penapisan automatik untuk kandungan dewasa                |
| CleanBrowsing Family Filter   | `185.228.168.168`<br>`185.228.169.168` | `2a00:5a60::bad1:0ff`<br>`2a00:5a60::bad2:0ff`   | Penapisan ketat, menyekat kandungan dewasa, proxy dan VPN |
| AdGuard DNS Family Protection | `94.140.14.15`<br>`94.140.15.16`       | `2a10:50c0::bad1:ff`<br>`2a10:50c0::bad2:ff`     | Menyekat iklan, tracker dan kandungan dewasa              |


### Kepentingan IPv6 dalam DNS

**Prevent bypass**
Banyak peranti moden menggunakan IPv6 secara default. Jika hanya DNS IPv4 ditetapkan, peranti boleh memintas penapisan melalui IPv6.

**Perlindungan menyeluruh**
Dengan menggunakan IPv4 dan IPv6 sekali, semua permintaan DNS akan ditapis.

**Prestasi**
IPv6 kadang-kadang memberi routing dan pemprosesan paket yang lebih pantas.

### Tips konfigurasi

* Tetapkan kedua-dua IPv4 dan IPv6.
* Konfigurasi pada router supaya seluruh rangkaian menggunakan DNS yang sama.


## Hosts File

Community list:
[https://github.com/4skinSkywalker/Anti-Porn-HOSTS-File/](https://github.com/4skinSkywalker/Anti-Porn-HOSTS-File/)

Hosts file hanya boleh menyekat domain penuh (top-level domain). Ia tidak boleh menyekat halaman tertentu.

Contoh:
Jika `twitter.com` disekat dalam hosts file, seluruh laman Twitter akan disekat. Jika hanya mahu sekat satu akaun atau satu halaman tertentu, hosts file tidak sesuai.

Untuk menyekat URL spesifik, gunakan browser extension.

### Cara edit hosts file (Windows)

1. Klik kanan pada ikon Notepad kemudian pilih **Run as administrator**
2. Pergi ke **File → Open**
3. Masukkan path berikut:

```
C:\windows\system32\drivers\etc\hosts
```

4. Tambah baris baru dan tampal kandungan dari `HOSTS.txt`
5. Simpan fail
6. Reboot komputer


## Guna Brave Browser

### Secure DNS

Tidak digalakkan.

Jika menggunakan DNS dalam tetapan Brave, penapisan hanya berlaku dalam Brave sahaja. Browser lain masih boleh membuka laman yang tidak ditapis.

Lebih baik tetapkan DNS di peringkat sistem atau router.

### Enable Content Filtering

Pergi ke *Settings -> Shields -> Content filtering*. Aktifkan *Porn Blocker*. 

Tambahkan custom filter list:

Lihat laman ini untuk cara konfigurasi: https://oisd.nl/setup/brave

Beberapa senarai tambahan:

```
https://big.oisd.nl  --> List besar
https://small.oisd.nl
https://nsfw.oisd.nl  --> List besar
https://nsfw-small.oisd.nl
```

## Browser Extension

Website Blocker:
[https://chromewebstore.google.com/detail/website-blocker/kniediipngcpmbmpeacaoinhoeipfina](https://chromewebstore.google.com/detail/website-blocker/kniediipngcpmbmpeacaoinhoeipfina)

Terdapat banyak extension yang boleh digunakan. Mana-mana pun boleh selagi ia menyokong fungsi menyekat URL.

Extension sesuai digunakan untuk:

* menyekat halaman tertentu
* menyekat URL spesifik
* menyekat profil pengguna tertentu dalam sesuatu laman

## SafeSearch Enforcement

Banyak enjin carian mempunyai mod penapisan kandungan dewasa yang dipanggil SafeSearch. Jika tidak dipaksa (enforced), pengguna masih boleh mematikannya.

Beberapa servis DNS family biasanya sudah memaksa SafeSearch secara automatik.

Contoh enjin carian yang menyokong SafeSearch:

* Google SafeSearch
* Bing SafeSearch
* YouTube Restricted Mode

Jika menggunakan router atau DNS yang menyokongnya, SafeSearch boleh dipaksa supaya tidak boleh dimatikan oleh pengguna.

Kelebihan kaedah ini:

* mengurangkan imej atau video lucah dalam hasil carian
* sesuai untuk peranti kanak-kanak
* berfungsi tanpa perlu memasang apa-apa pada browser


## Router-Level Blocking

Router boleh digunakan sebagai lapisan penapisan utama kerana semua peranti dalam rangkaian akan melalui router tersebut.

Beberapa fungsi yang boleh digunakan pada router:

* DNS filtering
* domain blacklist
* jadual akses internet
* parental control

Kelebihan konfigurasi di router:

* semua peranti dalam rumah ditapis secara automatik
* termasuk telefon, tablet, TV dan konsol permainan
* pengguna biasa tidak boleh ubah tetapan dengan mudah

Jika router menyokong firmware seperti:

* OpenWRT
* pfSense
* OPNsense

fungsi penapisan boleh menjadi lebih kuat.


## Block DNS Over HTTPS (DoH)

Browser moden boleh menggunakan DNS Over HTTPS (DoH). Ini membolehkan browser memintas DNS yang telah ditetapkan pada sistem.

Jika tidak dikawal, pengguna boleh bypass DNS filtering.

Langkah yang boleh dibuat:

* disable DoH dalam browser
* block DoH endpoint di router
* gunakan firewall rule untuk paksa penggunaan DNS router

Contoh domain DoH yang biasa digunakan:

* `dns.google`
* `cloudflare-dns.com`
* `mozilla.cloudflare-dns.com`


## Device-Level Parental Control

Sesetengah sistem operasi mempunyai kawalan ibu bapa terbina dalam.

Contoh:

Windows

* Microsoft Family Safety

Android

* Google Family Link

iOS

* Screen Time / Content Restrictions

Fungsi yang biasanya tersedia:

* sekat aplikasi tertentu
* sekat laman web tertentu
* had masa penggunaan peranti
* laporan aktiviti internet

Ini menambah satu lagi lapisan jika pengguna cuba memintas penapisan rangkaian.


## Gunakan Akaun Tanpa Admin

Jika peranti digunakan oleh kanak-kanak atau pengguna biasa, jangan beri akses admin.

Sebab:

* mereka tidak boleh ubah DNS
* tidak boleh install VPN
* tidak boleh ubah hosts file
* tidak boleh uninstall extension penapis

Gunakan akaun standard pada komputer.

## Kesimpulan

Untuk penapisan yang lebih kuat, gunakan beberapa lapisan serentak:

1. DNS filtering (router level)
2. IPv4 + IPv6 configuration
3. SafeSearch enforcement
4. hosts file (optional)
5. browser filtering
6. URL blocker extension
7. router firewall rules
8. device parental control

Gabungan kaedah ini memberikan penapisan yang lebih konsisten di seluruh rangkaian. Semakin banyak lapisan, semakin sukar untuk memintas sistem penapisan.