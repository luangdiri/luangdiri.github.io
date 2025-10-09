---
title: 'Juno App'
date: 2025-10-09 00:18:20
updated_at: 2025-10-09 16:30:16
tags:
- apps
---

Jarang dengar lelaki buat jurnal aktiviti seharian. Tapi itu lah aku, sejak zaman sekolah lagi aku dah mula tulis jurnal setiap hari. Bermula dari buku fizikal yang aku tulis pakai tangan, sehingga lah berpindah ke format digital sekarang.

*P/s: Aku guna perkataan "jurnal" sebab "diari" tu macam terlalu feminin. Tapi ya, secara teknikal nya memang aku ada tulis diari setiap hari.*

Sejarah nya, waktu sekolah dulu aku suka dengan konsep kapsul masa. Di mana aku set satu tarikh di masa hadapan lepas tu letak mesej yang aku nak baca waktu tu. Dari situ idea untuk buat jurnal pun terjebak lah. Maka aku pakai satu buku untuk tulis apa aku buat dalam hari tu. Sampai lah ke hari ini, tahun 2025 aku masih lagi menulis aktiviti harian aku. Cuma sekarang pakai keyboard lah.

Sebelum aku bina apps aku sendiri untuk tulis diari ni, aku cuma pakai notepad je. Kalau dalam phone, aku pakai mana-mana note taking app yang ada dekat pasaran waktu itu. Cerita tentang migrasi nota aku ada di [sini](1).

Oleh sebab aku peduli tentang privasi, aku pun mulakan langkah untuk membina app sendiri. Takut pakai app orang ni dia boleh baca diari kita, itu yang aku buat sendiri. Maka terlahir lah, **Juno**.

## Juno Electron (desktop)

Nama Juno ni berasal dari perkataan journal. Dari journal jadi lah Juno.

Versi pertama app ni aku bina pakai Electron. Vuejs untuk framework UI. Backend nya pakai Express framework dan MySQL untuk database. Tak ada mobile app, cuma ada desktop je. Jadi kurang convenient la dekat situ. 

<img width="60%" src="https://i.imgur.com/CHrVSCw.png"/>

**Source code:**

- Juno Electron (client): [https://github.com/aemxn/juno-ui](2)
- Juno Electron (backend): [https://github.com/aemxn/juno-server](3)

Antara feature yang ada dalam app ni adalah:

1. Create, update dan delete diary
2. Timeline view berdasarkan tahun dan bulan
3. Search diary entry
4. Export database (ke .sql)
5. Pagination

Aku tahu Electron framework ni boleh cross platform, tapi aku tak bina dengan plan untuk buat mobile app nya. Maka setiap hari kena buka laptop untuk tulis diari. Kadang tu aku dah tulis dulu dalam phone, lepas tu baru pindah ke desktop. Leceh.

Satu benda aku kurang gemar tentang app ni adalah ia rasa berat. Kali pertama kot bina app note taking ni. Semua benda pun aku nak sumbat, maka dia jadi bloated. Actually takde la bloated sangat, cuma dalam otak aku, aku expect app ni simple je: asalkan boleh CRUD dan export.

So projek ni pun terbengkalai akibat kurang memuaskan.

## Juno Android (mobile)



[1]: https://luangdiri.github.io/2024/02/03/migrasi-nota.html
[2]: https://github.com/aemxn/juno-ui
[3]: https://github.com/aemxn/juno-server
[4]: https://i.imgur.com/CHrVSCw.png