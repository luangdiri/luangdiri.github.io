---
title: "Migrasi Nota"
date: 2024-02-04 00:28:31 +0800
tags:
- meta
---

# Migrasi Nota

Akhirnya, aku berjaya centralize kan segala nota dan tulisan aku dalam satu folder.

![qownnotes][qownnotes]

Sebelum ni bersepah disebabkan banyak bertukar-tukar app. Seingat aku masa mula-mula aku dapat phone Android, aku pakai Gnotes app untuk tulis nota dan fikiran rawak aku. Lepas tu bila Gnotes buat hal (kena bayar), aku tukar ke Evernote. App ni bloated gila. Banyak fungsi yang aku tak perlu. Selepas tu pakai Standard Notes pula. Di antara tu ada juga aku cari-cari note taking app yang simple. Ada juga jumpa, cuma tak menepati citarasa.

Akhirnya aku terjumpa satu app ni nama nya [Zettel Notes](https://play.google.com/store/apps/details?id=org.eu.thedoc.zettelnotes&hl=en&gl=US). Aku cuma nak dua fungsi ni je 1. Markdown support, 2. boleh sync ke Google Drive atau mana-mana cloud service. Yang paling baik boleh sync ke server sendiri lah. Cuma aku takde server sendiri, jadi pakai je servis awan mana-mana.

Dari sudut positifnya, app-app yang aku pakai tu semua ada fungsi untuk mengeksport nota. Bila aku dah taknak pakai Gnotes tu, aku eksport semua. Cuma masalahnya dia eksport dalam format HTML. Aku nak Markdown, supaya senang, semua nota aku dalam Markdown.

Jadi sini aku cerita perjalanan aku migrasi segala nota-nota dan blog legasi aku ke format Markdown.

- **Standard Notes**: eksport ke plaintext, tak perlu buat apa-apa.
- **Evernote**: tulis batch script untuk convert HTML ke Markdown.
- **Gnotes**: tulis batch script untuk convert HTML ke Markdown.
- **Juno diary**: tulis javascript untuk convert JSON ke Markdown.
- **Blog 1**: blog pertama aku tahun 2011 pakai Blogspot. Eksport format dalam XML.
- **Blog 2**: blog kedua aku tahun 2014 juga pakai Blogspot. Eksport format dalam XML.
- **Knowledge Log blog**: blog ketiga circa 2015, pakai Ghost. Eksport format dalam JSON.
- **aemxn blog**: blog keempat circa 2020 ke atas, pakai static site generator (11ty). Tak perlu eksport, dah memang dalam Markdown.

### 1. Evernote

Evernote eksport nota nya ke dalam format HTML. Jadi aku kena cari cara nak convert HTML ke Markdown. Ini pertama kali aku buat benda ni. Biasanya aku Google dengan keyword *"how to convert html to markdown"*, tapi zaman dah berubah. Kita ada AI sekarang 😎

Ya, aku pakai ChatGPT. Ini juga pertama kali aku pakai ChatGPT ni untuk tolong aku dalam programming. Maksud aku dalam skala serius macam ni lah. Tak sangka jawapan dia sangat-sangat membantu. Cuma ada cacat cela sikit, kena manual betulkan.

Bawah ni adalah batch script untuk loop semua file HTML yang ada dalam folder, dan convert file tersebut ke dalam format Markdown. Untuk file conversion, kita pakai library [Pandoc](https://pandoc.org/).

```powershell
@echo off

rem Loop through each HTML file and convert to Markdown
for %%i in (*.html) do (
    pandoc "%%i" --from html --to markdown_strict -o "%%~ni.md"
)
```

Dalam masa yang sama, aku dapat belajar yang expression macam ni: `"%%~ni"` adalah untuk dapatkan filename sahaja, tanpa extension file tersebut.

> Baca chat penuh bersama ChatGPT: [https://chat.openai.com/share/dab2d115-7f67-4764-8c04-e715e68f710e](https://chat.openai.com/share/dab2d115-7f67-4764-8c04-e715e68f710e)

### 2. Gnotes

Untuk GNotes pula masalah nya berbeza dengan Evernote. Struktur pengeksportan nya ada folder dan dalam folder tersebut ada nota dalam bentuk HTML, yang dinamakan `content.html`.

Nama file semua sama, cuma nama folder je lain. Nama folder nya tak ikut title yang kita berikan dalam nota tu, tapi dia pakai hash.

Jadi solusi aku,
1. Tukar nama file `content.html` tu kepada nama folder yang dia berada
2. Copy file tersebut ke atas satu aras direktori
3. Convert semua file yang dipindah tadi ke format Markdown guna Pandoc

GNotes punya struktur eksport:

```plaintext
~
│   html2md.bat
│   rename-move.bat
├───5758c3a843b8b46ba2444776
│       content.html
│
├───5759d18d2f2e8d8043eb9dc9
│       content.html
│
├───57b0ac1cf8d0db34d6a357ef
│       content.html
│
├───760c2931-0910-40cb-a112-38835bdefc6a
│       content.html
│
└───f7e21221-4473-474c-8751-cbdcefb2bebf
        content.html
```

Untuk proses `rename` and `copy` aku jalankan berasingan dengan Pandoc, untuk senang kawal keadaan. Takut jadi barai kalau aku masuk semua script dalam satu.

Juga pakai ChatGPT, berikut adalah batch script penuh untuk `rename` dan `copy`:

```powershell
@echo off
setlocal enabledelayedexpansion

set "sourceFolder=%cd%"

for /d %%i in ("%sourceFolder%\*") do (
    set "folderName=%%~nxi"
    set "fileName=content.html"
    set "newFileName=!folderName!.html"
    
    ren "%%i\!fileName!" "!newFileName!"
    copy "%%i\!newFileName!" "%sourceFolder%"
)

echo Done!
```

> Chat penuh disini: [https://chat.openai.com/share/8cd06f97-4b54-49d2-9a24-382c688373e9](https://chat.openai.com/share/8cd06f97-4b54-49d2-9a24-382c688373e9)

### 3. Juno

Juno ni adalah diari aku yang aku tulis dalam app yang aku bina dulu. App tu disambungkan ke MySQL untuk simpan diari-diari tu semua. Aku juga dah buat fungsi eksport ke dalam JSON untuk tujuan backup.

Untuk masalah ni, aku selesaikan dengan tulis Javascript untuk parse JSON tersebut dan masukkan isi nya ke dalam fail Markdown. Btw, Markdown ni plaintext biasa je, cuma extension je `.md` dan boleh di baca oleh Markdown viewer.

Ini adalah struktur JSON diari tu:

```json
{
  "entries": [
    {
      "id": 1,
      "title": "From OhLife (November 28, 2014)",
      "date": "2014-12-07",
      "body": "\\nkajfsdbfkjabsdfkjadbsfkjasdnfknasdfk.\\nisandfkja we\\'re...",
      "createdAt": "2020-07-11T22:04:14.000Z",
      "updatedAt": "2020-07-11T22:04:14.000Z"
    },
    {
      "id": 2,
      "title": "Title 2",
      "date": "2014-12-08",
      "body": "Lorem ipsum dolor sit amet, ....",
      "createdAt": "2020-07-11T22:04:14.000Z",
      "updatedAt": "2020-07-11T22:04:14.000Z"
    },
    {
      "id": 2999,
      "title": "AD 2172.2023",
      "date": "2023-03-16",
      "body": "The standard chunk of...",
      "createdAt": "2023-03-16T17:19:49.000Z",
      "updatedAt": "2023-03-16T17:19:49.000Z"
    }
  ]
}
```

Macam biasa, ChatGPT membantu aku menyiapkan kerja dengan pantas. Javascript bawah ni kerja dia adalah,

1. Baca JSON file
2. Loop array yang ada dalam tu
3. Parse isi nya
4. Create folder baru dalam format `{tahun}/{bulan}`
5. Write file tersebut ke dalam folder sesuai tahun dan bulan mengikut field date entri diari tersebut
6. ???
7. Profit!!1


```js
const fs = require('fs');

// Read JSON data from external file
const jsonData = JSON.parse(fs.readFileSync('sample.json', 'utf-8'));

// Iterate over each entry and store formatted content in a separate Markdown file
jsonData.entries.forEach(entry => {
    const originalTitle = entry.title;
    const date = new Date(entry.date);
    const body = unescape(entry.body); // Unescape the body field

    // Format the date as "DD.MM.YYYY"
    const day = String(date.getDate()).padStart(2, '0');
    const month = String(date.getMonth() + 1).padStart(2, '0'); // Months are 0-indexed in JavaScript
    const year = date.getFullYear();
    const formattedDate = `${day}.${month}.${year}`;

    // Create formatted title as "AD YYYY.MM.DD"
    const formattedTitle = `AD ${formattedDate}`;

    // Create a folder for the year and month
    const folderPath = `./${year}/${month}`;

    if (!fs.existsSync(folderPath)) {
        fs.mkdirSync(folderPath, { recursive: true });
        console.log(`Created folder: ${folderPath}`);
    }

    // Use formatted title as the filename with .md extension
    const filename = `${folderPath}/${formattedTitle}.md`;

    // Write Markdown content to file
    const markdownContent = `# ${originalTitle}\n\nDate: ${formattedDate}\n\n${body}`;

    // Remove '\n' characters from the body
    const cleanedMarkdown = markdownContent.replace(/\\n/g, ' ').replace(/\\'/g, '\'');

    fs.writeFileSync(filename, cleanedMarkdown, 'utf-8');

    console.log(`Markdown file created: ${filename}`);
});

```

Untuk rekod, cara nak run benda ni:

```
$ npm init -y
$ npm install fs
$ npm index.js
```

> Baca chat penuh bersama ChatGPT: [https://chat.openai.com/share/9c444ee7-865e-4af4-b977-076dadda9b0b](https://chat.openai.com/share/9c444ee7-865e-4af4-b977-076dadda9b0b)

### 4. Blogspot

Blogspot eksport data nya ke dalam XML. Format lapuk untuk pengeksportan. Disebabkan Blogspot ni dulu popular dan ramai guna, dah ada orang buat tool untuk convert Blogger (XML) ke Markdown. Maka mudah lah kerja aku. 

[blog2md](https://github.com/palaniraja/blog2md) (nodejs)

### 5. Ghost

Ghost pula eksport dalam JSON, cuma dia punya struktur JSON agak berterabur. Susah aku nak baca. Sama kes macam Blogspot tadi, ia popular dan ramai guna. Mujur sekali ada juga orang baik yang buat tool untuk convert Ghost post ke Markdown.

[ghost-to-md](https://github.com/hswolff/ghost-to-md) (nodejs)

### 6. Telegram

Ni aku belum buat lagi. Aku ada juga tulis nota dalam Telegram. Telegram juga ada fungsi eksport ke HTML dan JSON. Tadi aku Google, dah ada tool untuk convert JSON ni ke Markdown. Syukur, ramai orang bijak dan baik dalam dunia teknologi ni. Mungkin nanti la aku pakai tool ni sebab kena install Python.

[telegram-to-markdown](https://github.com/alexlyzhov/telegram-to-markdown) (python)

### Markdown viewer

Lepas dah convert segala benda ke Markdown, maka senang lah aku nak centralize kan semua nota tersebut ke dalam satu tempat. Dengan menggunakan software untuk view Markdown file, dapatlah aku baca semua nota tu dengan hati yang senang.

Ada beberapa software yang boleh digunakan untuk tujuan menyusun, mengurus, menyunting dan melihat Markdown file. Aku sekarang pakai [QOwnNotes](https://www.qownnotes.org/). Dia sumber terbuka dan ada fitur yang aku nak selama ni iaitu ada kebolehan untuk imbas sub-direktori. Jadi tak perlu aku nak buat tag bagai.

Kalau sebelum ni aku pakai [Notable](https://notable.app/). Bagus juga, cuma nak menyusun nota kena pakai tag dan dia tak boleh imbas sub-direktori. Jadi aku terpaksa tinggalkan ia.

Untuk sekadar nak view file Markdown, aku pakai [mdview](https://github.com/c3er/mdview/), macam notepad.exe tapi untuk Markdown.

[qownnotes]: https://raw.githubusercontent.com/luangdiri/luangdiri.github.io/main/_media/qownnotes.png