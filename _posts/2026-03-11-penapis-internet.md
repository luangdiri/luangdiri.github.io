---
title: 'Penapis Internet'
date: 2026-03-11 00:35:10
updated_at: 2026-04-06 23:45:00
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
- [DNS Untuk Keluarga](#dns-untuk-keluarga)
  - [Setting Untuk PC (Windows)](#setting-untuk-pc-windows)
  - [Setting untuk Android](#setting-untuk-android)
  - [Setting untuk iOS](#setting-untuk-ios)
- [Hosts File](#hosts-file)
  - [Cara edit hosts file (Windows)](#cara-edit-hosts-file-windows)
- [Guna Brave Browser](#guna-brave-browser)
  - [Secure DNS](#secure-dns)
  - [Content Filtering](#content-filtering)
  - [Brave "Block Elements"](#brave-block-elements)
- [Browser Extension](#browser-extension)
- [SafeSearch Enforcement](#safesearch-enforcement)
- [Router-Level Blocking](#router-level-blocking)
- [Block DNS Over HTTPS (DoH)](#block-dns-over-https-doh)
- [Device-Level Parental Control](#device-level-parental-control)
- [Gunakan Akaun Tanpa Admin](#gunakan-akaun-tanpa-admin)
- [Kesimpulan](#kesimpulan)

## DNS Untuk Keluarga

### Setting Untuk PC (Windows)

Family DNS ialah servis DNS yang sudah siap dengan penapisan kandungan seperti malware, iklan berbahaya, dan laman dewasa.

Menetapkan DNS secara manual pada PC memastikan peranti sentiasa menggunakan servis penapisan yang dipilih, walaupun tidak bergantung kepada router.

1. Buka **Settings**
2. Pergi ke **Network & Internet**
3. Pilih **Advanced network settings**
4. Klik **More network adapter options**
5. Klik kanan pada network yang digunakan → **Properties**
6. Pilih **Internet Protocol Version 4 (IPv4)** → **Properties**
7. Pilih **Use the following DNS server addresses**
8. Masukkan DNS pilihan (contoh: `1.1.1.3` dan `1.0.0.3`)
9. Ulang langkah untuk **Internet Protocol Version 6 (IPv6)** jika perlu
10. Klik **OK**

**Nota**

* Pastikan set kedua-dua IPv4 dan IPv6 untuk elak bypass
* Tetapan ini hanya terpakai pada PC tersebut
* Boleh digunakan sebagai backup jika router tidak dikonfigurasi

![ipv4](https://i.imgur.com/lgVAUxk.png)

| DNS                           | IPv4                                   | IPv6                                             | Description                                               |
| ----------------------------- | -------------------------------------- | ------------------------------------------------ | --------------------------------------------------------- |
| Cloudflare for Families       | `1.1.1.3`<br>`1.0.0.3`                 | `2606:4700:4700::1113`<br>`2606:4700:4700::1003` | Menyekat malware dan kandungan dewasa                     |
| OpenDNS FamilyShield          | `208.67.222.123`<br>`208.67.220.123`   | `2620:0:ccc::2`<br>`2620:0:ccd::2`               | Penapisan automatik untuk kandungan dewasa                |
| CleanBrowsing Family Filter   | `185.228.168.168`<br>`185.228.169.168` | `2a00:5a60::bad1:0ff`<br>`2a00:5a60::bad2:0ff`   | Penapisan ketat, menyekat kandungan dewasa, proxy dan VPN |
| AdGuard DNS Family Protection | `94.140.14.15`<br>`94.140.15.16`       | `2a10:50c0::bad1:ff`<br>`2a10:50c0::bad2:ff`     | Menyekat iklan, tracker dan kandungan dewasa              |


**Kepentingan IPv6 dalam DNS:**

* Prevent bypass: Banyak peranti moden menggunakan IPv6 secara default. Jika hanya DNS IPv4 ditetapkan, peranti boleh memintas penapisan melalui IPv6.
* Perlindungan menyeluruh: Dengan menggunakan IPv4 dan IPv6 sekali, semua permintaan DNS akan ditapis.
* Prestasi: IPv6 kadang-kadang memberi routing dan pemprosesan paket yang lebih pantas.

**Tips konfigurasi**

* Tetapkan kedua-dua IPv4 dan IPv6.
* Konfigurasi pada router supaya seluruh rangkaian menggunakan DNS yang sama.

### Setting untuk Android

Selain tetapan DNS menggunakan IP (IPv4/IPv6), Android menyokong penggunaan **Private DNS** melalui hostname (DNS over TLS). Ini lebih konsisten kerana semua trafik DNS akan dipaksa melalui endpoint tersebut.

Contoh penggunaan:

* Buka **Settings**
* Pergi ke **Network & Internet** (atau Connection settings, bergantung pada peranti)
* Pilih **Private DNS**
* Pilih **Private DNS provider hostname**
* Masukkan hostname DNS

**DNS Hostname**

| DNS                           | Hostname                              |
| ----------------------------- | ------------------------------------- |
| Cloudflare for Families       | `family.cloudflare-dns.com`           |
| CleanBrowsing Family Filter   | `family-filter-dns.cleanbrowsing.org` |
| AdGuard DNS Family Protection | `family.adguard-dns.com`              |

**Nota:**

* OpenDNS FamilyShield tidak menyediakan hostname untuk Private DNS (hanya IP).
* Tetapan ini hanya terpakai pada peranti tersebut, tidak seperti router-level DNS.
* Sesuai digunakan jika tiada akses kepada router atau sebagai lapisan tambahan.

### Setting untuk iOS

iOS tidak menyediakan tetapan Private DNS secara langsung seperti Android. Sebaliknya, ia menggunakan **configuration profile** untuk menetapkan DNS over HTTPS (DoH) atau DNS over TLS (DoT).

Kaedah ini tetap mencapai tujuan yang sama: memaksa semua trafik DNS melalui servis penapisan.

**Cara Setup**

1. Muat turun configuration profile dari penyedia DNS
2. Buka **Settings**
3. Pergi ke **Profile Downloaded**
4. Install profile tersebut
5. Aktifkan DNS melalui profile yang dipasang

**DNS Configuration Profile**

| DNS                           | Setup                                                                                                        |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------ |
| Cloudflare for Families       | [https://developers.cloudflare.com/1.1.1.1/setup/ios/](https://developers.cloudflare.com/1.1.1.1/setup/ios/) |
| CleanBrowsing Family Filter   | [https://cleanbrowsing.org/help/docs/ios/](https://cleanbrowsing.org/help/docs/ios/)                         |
| AdGuard DNS Family Protection | [https://adguard-dns.io/en/public-dns.html](https://adguard-dns.io/en/public-dns.html)                       |

**Nota:**

* Profile ini akan override DNS default sistem.
* Lebih sukar untuk dipintas berbanding tetapan biasa.
* Boleh digunakan bersama DNS di router untuk perlindungan tambahan.


## Hosts File

Community list:
[https://github.com/4skinSkywalker/Anti-Porn-HOSTS-File/](https://github.com/4skinSkywalker/Anti-Porn-HOSTS-File/)

Hosts file hanya boleh menyekat domain penuh (top-level domain). Ia tidak boleh menyekat halaman tertentu.

Contoh:  
Jika **twitter.com** disekat dalam hosts file, seluruh laman Twitter akan disekat. Jika hanya mahu sekat satu akaun atau satu halaman tertentu, hosts file tidak sesuai.

Untuk menyekat URL spesifik, gunakan browser extension.

### Cara edit hosts file (Windows)

1. Klik kanan pada ikon Notepad kemudian pilih **Run as administrator**
2. Pergi ke **File → Open**
3. Masukkan path berikut:

```
C:\windows\system32\drivers\etc\hosts
```

4. Tambah baris baru dan tampal kandungan dari `hosts.txt`
5. Simpan fail
6. Reboot komputer


## Guna Brave Browser

### Secure DNS

Tidak digalakkan.

Jika menggunakan DNS dalam tetapan Brave, penapisan hanya berlaku dalam Brave sahaja. Browser lain masih boleh membuka laman yang tidak ditapis.

Lebih baik tetapkan DNS di peringkat sistem atau router.

### Content Filtering

Pergi ke *Settings -> Shields -> Content filtering*. Aktifkan *Porn Blocker*. 

Tambahkan custom filter list:

Lihat laman ini untuk cara konfigurasi: https://oisd.nl/setup/brave

Beberapa senarai tambahan:

- https://big.oisd.nl  --> List besar
- https://small.oisd.nl
- https://nsfw.oisd.nl  --> List besar
- https://nsfw-small.oisd.nl

### Brave "Block Elements"

![block elements](https://i.imgur.com/K25A5Nn.jpg)

Brave mempunyai fungsi **Block Elements** yang membolehkan kita menyembunyikan elemen tertentu dalam sesuatu laman web menggunakan custom filter (CSS selector).

Berbeza dengan DNS atau hosts file yang menyekat seluruh domain, fungsi ini beroperasi pada peringkat halaman. Ia sesuai untuk kes di mana laman web masih diperlukan, tetapi hanya bahagian tertentu sahaja yang ingin ditapis.

Dalam konteks penapisan kandungan, kaedah ini boleh digunakan untuk:

* membuang elemen distraksi seperti Reels, Story
* menyekat sponsored ads
* mengurangkan pendedahan kepada kandungan tidak sesuai dalam social media
* meningkatkan fokus semasa melayari web

Contoh penggunaan adalah untuk Facebook dan Instagram (versi web), dengan menyekat elemen tertentu yang sering memaparkan kandungan tidak ditapis. Di bawah ini adalah filter yang aku pakai untuk menyekat elemen seperti Reel dan Story dalam Facebook dan Instagram (web).

```
www.facebook.com##.xb57i2i.x1q594ok.x5lxg6s.x78zum5.xdt5ytf.x10wlt62.x1n2onr6.x1ja2u2z.x1pq812k.xfk6m8.x1yqm8si.xjx87ck.xw2csxc.x7p5m3t.x9f619.xat24cr.xwib8y2.x1y1aw1k.x1rohswg.xhfbhpw > .x78zum5.x1iyjqo2.x1n2onr6.x1q0g3np
www.facebook.com##.x1n2onr6.xwya9rg.x1y1aw1k.xwib8y2.x4vbgl9
www.facebook.com##.x9f619.x1ja2u2z.xnp8db0.x112wk31.xnjgh8c.xxc7z9f.x1t2pt76.x1u2d2a2.x6ikm8r.x10wlt62.x7wzq59.xxzkxad.x1daaz14 > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.xedcshv.x1t2pt76 > .xb57i2i.x1q594ok.x5lxg6s.x78zum5.xdt5ytf.x6ikm8r.x1ja2u2z.x1pq812k.x1rohswg.xfk6m8.x1yqm8si.xjx87ck.x1l7klhg.x1iyjqo2.xs83m0k.x2lwn1j.xx8ngbg.xwo3gff.x1oyok0e.x1odjw0f.x1e4zzel.x1n2onr6.xq1qtft > div.x78zum5.xdt5ytf.x1iyjqo2.x1n2onr6:nth-of-type(1) > div.x1y1aw1k:nth-of-type(1) > div:nth-of-type(1) > div:nth-of-type(1) > div:nth-of-type(1) > .x1n2onr6.x1uc6qws.xyen2ro > div.xwib8y2.x1y1aw1k:nth-of-type(2)
www.facebook.com##.x9f619.x1ja2u2z.xnp8db0.x112wk31.xnjgh8c.xxc7z9f.x1t2pt76.x1u2d2a2.x6ikm8r.x10wlt62.x7wzq59.xxzkxad.x1daaz14 > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.xedcshv.x1t2pt76 > .xb57i2i.x1q594ok.x5lxg6s.x78zum5.xdt5ytf.x6ikm8r.x1ja2u2z.x1pq812k.x1rohswg.xfk6m8.x1yqm8si.xjx87ck.x1l7klhg.x1iyjqo2.xs83m0k.x2lwn1j.xx8ngbg.xwo3gff.x1oyok0e.x1odjw0f.x1e4zzel.x1n2onr6.xq1qtft > div.x78zum5.xdt5ytf.x1iyjqo2.x1n2onr6:nth-of-type(1) > div.x1y1aw1k:nth-of-type(1) > div:nth-of-type(1) > div:nth-of-type(1) > div:nth-of-type(1) > .x1n2onr6.x1uc6qws.xyen2ro > .x1n2onr6.x1ja2u2z.x9f619.x78zum5.xdt5ytf.x2lah0s.x193iq5w.xjkvuk6.x1cnzs8 > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.x1iyjqo2.x2lwn1j > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.x2lah0s.x193iq5w.xf7dkkf.xv54qhq > .x78zum5.xdt5ytf.xz62fqu.x16ldp7u > .xu06os2.x1ok221b > .x193iq5w.xeuugli.x13faqbe.x1vvkbs.x1xmvt09.x1lliihq.x1s928wv.xhkezso.x1gmr53x.x1cpjm7i.x1fgarty.x1943h6x.xudqn12.x676frb.x1jchvi3.x1lbecb7.x1s688f.xzsf02u > .x9f619.x1ja2u2z.x78zum5.x2lah0s.x1n2onr6.x1qughib.x6s0dn4.xozqiw3.x1q0g3np.xzt5al7 > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.x193iq5w.xeuugli.x1r8uery.x1iyjqo2.xs83m0k > .html-h3.xdj266r.x14z9mp.xat24cr.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x1vvkbs.x1heor9g.x1qlqyl8.x1pd3egz.x1a2a7pz.x193iq5w.xeuugli > .x193iq5w.xeuugli.x13faqbe.x1vvkbs.x1xmvt09.x1jchvi3.x1lbecb7.x1s688f.xi81zsa > .x1lliihq.x6ikm8r.x10wlt62.x1n2onr6.x1j85h84
www.facebook.com##li.html-li.xdj266r.x14z9mp.xat24cr.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl:nth-of-type(8) > div.x1n2onr6.x1ja2u2z.x9f619.x78zum5.xdt5ytf.x2lah0s.x193iq5w:nth-of-type(1) > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.x1iyjqo2.x2lwn1j > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.x2lah0s.x193iq5w.xmzvs34.xf159sx > .x1i10hfl.x1qjc9v5.xjbqb8w.xjqpnuy.xc5r6h4.xqeqjp1.x1phubyo.x13fuv20.x18b5jzi.x1q0q8m5.x1t7ytsu.x972fbf.x10w94by.x1qhh985.x14e42zd.x9f619.x1ypdohk.xdl72j9.x2lah0s.x3ct3a4.xdj266r.x14z9mp.xat24cr.x1lziwak.x2lwn1j.xeuugli.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x1n2onr6.x16tdsg8.x1hl2dhg.xggy1nq.x1ja2u2z.x1t137rt.x1fmog5m.xu25z0z.x140muxe.xo1y3bh.x1q0g3np.x87ps6o.x1lku1pv.x1a2a7pz.x1lliihq > .html-div.x1qjc9v5.x9f619.x78zum5.xdt5ytf.x1iyjqo2.xl56j7k.xeuugli.xifccgj.x4cne27.xw01apr.x1ws5yxj.xbktkl8.x1tr5nd9.x3su7b9.x12pbpz1.x1gtkyd9.x1r8uycs > .x9f619.x1ja2u2z.x78zum5.x2lah0s.x1n2onr6.x1qughib.x6s0dn4.xozqiw3.x1q0g3np
www.facebook.com##li.html-li.xdj266r.x14z9mp.xat24cr.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl:nth-of-type(8) > div.x1n2onr6.x1ja2u2z.x9f619.x78zum5.xdt5ytf.x2lah0s.x193iq5w:nth-of-type(1) > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.x1iyjqo2.x2lwn1j > .x9f619.x1n2onr6.x1ja2u2z.x78zum5.xdt5ytf.x2lah0s.x193iq5w.xmzvs34.xf159sx > .x1i10hfl.x1qjc9v5.xjbqb8w.xjqpnuy.xc5r6h4.xqeqjp1.x1phubyo.x13fuv20.x18b5jzi.x1q0q8m5.x1t7ytsu.x972fbf.x10w94by.x1qhh985.x14e42zd.x9f619.x1ypdohk.xdl72j9.x2lah0s.x3ct3a4.xdj266r.x14z9mp.xat24cr.x1lziwak.x2lwn1j.xeuugli.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x1n2onr6.x16tdsg8.x1hl2dhg.xggy1nq.x1ja2u2z.x1t137rt.x1fmog5m.xu25z0z.x140muxe.xo1y3bh.x1q0g3np.x87ps6o.x1lku1pv.x1a2a7pz.x1lliihq > .html-div.x1qjc9v5.x9f619.x78zum5.xdt5ytf.x1iyjqo2.xl56j7k.xeuugli.xifccgj.x4cne27.xw01apr.x1ws5yxj.xbktkl8.x1tr5nd9.x3su7b9.x12pbpz1.x1gtkyd9.x1r8uycs
www.facebook.com##.x78zum5.x1q0g3np.x1qughib.xz9dl7a.xpdmqnj.x1120s5i.x1g0dm76
www.facebook.com##.x1hc1fzr.x1unhpq9.x6o7n8i > div:nth-of-type(1) > div:nth-of-type(2) > div.x1lliihq:nth-of-type(2) > div:nth-of-type(1) > span:nth-of-type(1) > div.x1lliihq > div:nth-of-type(1) > .x1n2onr6.xh8yej3.x1ja2u2z.xod5an3 > div.x1n2onr6.x1ja2u2z:nth-of-type(1) > div:nth-of-type(1) > div:nth-of-type(1) > div.x1a2a7pz:nth-of-type(1) > div.x78zum5.xdt5ytf:nth-of-type(1) > div.x9f619.x1n2onr6.x1ja2u2z:nth-of-type(1) > div.html-div.xdj266r.x14z9mp.xat24cr.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x78zum5.x1n2onr6.xh8yej3:nth-of-type(1) > div.x1n2onr6.x1ja2u2z.x1jx94hy.xw5cjc7.x1dmpuos.x1vsv7so.xau1kf4.x9f619.xh8yej3.x6ikm8r.x10wlt62.xquyuld:nth-of-type(1) > div:nth-of-type(1) > .html-div.xdj266r.x14z9mp.xat24cr.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x78zum5.x1n2onr6.xh8yej3 > .x1n2onr6.x1ja2u2z.x1jx94hy.xw5cjc7.x1dmpuos.x1vsv7so.xau1kf4.x9f619.xh8yej3.x6ikm8r.x10wlt62.xquyuld > div.x6ikm8r.x10wlt62:nth-of-type(1) > div:nth-of-type(2) > div.x9f619.x1n2onr6.x1ja2u2z:nth-of-type(1) > .x9f619.x1n2onr6.x1ja2u2z.x1wsgfga.x9otpla.xwib8y2.x1y1aw1k > .xb57i2i.x1q594ok.x5lxg6s.x78zum5.xdt5ytf.x10wlt62.x1n2onr6.x1ja2u2z.x1pq812k.xfk6m8.x1yqm8si.xjx87ck.xw2csxc.x7p5m3t.x9f619.xat24cr.xwib8y2.x1y1aw1k.x1rohswg.xhfbhpw
www.instagram.com##.x1i10hfl.x9f619.x1ypdohk.xdl72j9.x2lah0s.x3ct3a4.xdj266r.x14z9mp.xat24cr.x1lziwak.x2lwn1j.xeuugli.xexx8yu.x18d9i69.x16tdsg8.x1hl2dhg.xggy1nq.x1ja2u2z.x1t137rt.x1q0g3np.x87ps6o.x1lku1pv.x1a2a7pz.x6s0dn4.x7r02ix.x1ss9elp.x11ppq56.xvhwddo.x1o29io0.x13fuv20.x18b5jzi.x1q0q8m5.x1t7ytsu.x1w60jca.x3nfvp2.xl56j7k.x1n2onr6.x6zsckl.x10qfohq.xdwr3uu.xly64p6.x178xt8z.x1lun4ml.xso031l.xpilrb4.xnnlda6.xv54qhq.xf7dkkf.x1ddxa5k.xysibl7.x4yb96v.x1kylhsf.xed3198
www.instagram.com##.html-div.xdj266r.x14z9mp.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x9f619.xjbqb8w.x78zum5.x15mokao.x1ga7v0g.x16uus16.xbiv7yw.x1yztbdb.x1n2onr6.x1plvlek.xryxfnj.x1c4vz4f.x2lah0s.xdt5ytf.xqjyukv.x1qjc9v5.x1oa3qoh.x1nhvcw1 > .html-div.xdj266r.x14z9mp.xat24cr.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x9f619.xjbqb8w.x78zum5.x15mokao.x1ga7v0g.x16uus16.xbiv7yw.x1n2onr6.x1plvlek.xryxfnj.x1c4vz4f.x2lah0s.x1q0g3np.xqjyukv.x6s0dn4.x1oa3qoh.x1nhvcw1
www.instagram.com##.html-div.xdj266r.x14z9mp.xat24cr.x1lziwak.xyri2b.x1c1uobl.x9f619.xjbqb8w.x78zum5.x15mokao.x1ga7v0g.x16uus16.xbiv7yw.xwib8y2.x1y1aw1k.x1uhb9sk.x1plvlek.xryxfnj.x1c4vz4f.x2lah0s.xdt5ytf.xqjyukv.x1qjc9v5.x1oa3qoh.x1nhvcw1
www.instagram.com##.html-div.xdj266r.xat24cr.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x9f619.xjbqb8w.x78zum5.x15mokao.x1ga7v0g.x16uus16.xbiv7yw.xyqm7xq.x1ys307a.x1uhb9sk.x1plvlek.xryxfnj.x1c4vz4f.x2lah0s.xdt5ytf.xqjyukv.x1qjc9v5.x1oa3qoh.x1nhvcw1
www.instagram.com##.html-div.x14z9mp.xat24cr.x1lziwak.xexx8yu.xyri2b.x18d9i69.x1c1uobl.x9f619.xjbqb8w.x78zum5.x15mokao.x1ga7v0g.x16uus16.xbiv7yw.xseo6mj.x1uhb9sk.x1plvlek.xryxfnj.x1c4vz4f.x2lah0s.xdt5ytf.xqjyukv.x1qjc9v5.x1oa3qoh.x1nhvcw1
www.instagram.com##.x1iyjqo2.xdj266r.x11t971q.xat24cr.xvc5jky.x38y82z.x1xnnf8n.x18d9i69.x106a9eq.x1wfb79h.xb3f9ss.x1k4gc0v.x1hfk2xs.x10rn61k.x1m6xvf3.x12wacv3 > div:nth-of-type(1) > div:nth-of-type(2)
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

Fungsi penapisan boleh menjadi lebih kuat.

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