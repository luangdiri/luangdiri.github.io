---
title: "Perdagangan Algoritma - Bhg I"
date: 2021-08-21 21:28:55
lastmod: 2025-07-02 16:30:23
tags:
- tutorial
- trading
---

**Kandungan:**

- [1. Cari Strategi](#1-cari-strategi)
- [2. Berlatih Secara Manual Selama Sebulan](#2-berlatih-secara-manual-selama-sebulan)
- [3. Apa Itu Tingkah Laku Strategi?](#3-apa-itu-tingkah-laku-strategi)
- [4. Scripting (Penulisan Kod)](#4-scripting-penulisan-kod)
  - [4.1 Bahasa Apa Nak Guna?](#41-bahasa-apa-nak-guna)
- [5. Backtest di TradingView](#5-backtest-di-tradingview)
  - [5.1 Guna Alert untuk Uji Keberkesanan](#51-guna-alert-untuk-uji-keberkesanan)
  - [5.2 Pentingnya Faham Kelemahan Backtest](#52-pentingnya-faham-kelemahan-backtest)

---

Perdagangan algoritma atau **algorithm trading** adalah kaedah berdagang menggunakan kod yang ditulis untuk mengikuti strategi tertentu tanpa perlu campur tangan manusia. Untuk pedagang yang sukakan automasi, ini adalah satu jalan ke arah perdagangan konsisten dan efisien tanpa melibatkan emosi manusia. Dalam post kali ini, aku akan kongsikan langkah-langkah asas untuk bermula dengan algo trading berdasarkan pengalaman aku sendiri.

## 1. Cari Strategi

Langkah pertama sebelum fikir pasal coding, automasi, dan backtesting adalah mencari **strategi** yang berkesan. Strategi boleh jadi sesuatu yang mudah seperti **moving average crossover** atau RSI oversold/overbought, atau yang lebih kompleks seperti gabungan beberapa indikator teknikal, pattern price action, atau sentimen data.

Sebagai contoh, gabungan MA crossover dan RSI mungkin kelihatan seperti berikut:

    Buy Entry (Long Position):
    - MA 20 crosses above MA 50 (bullish crossover)
    - RSI is above 50 (momentum confirmation)

    Sell Entry (Short Position):
    - MA 20 crosses below MA 50 (bearish crossover)
    - RSI is below 50 (momentum confirmation)

    Exit:
    - Optional: opposite crossover or RSI reversion
    - For simplicity, exit on opposite signal

## 2. Berlatih Secara Manual Selama Sebulan

Sebelum kau automate strategi yang kau rasa boleh buat duit tu, latih dulu guna paper trade. Jangan terus masuk live trade yang pakai duit sebenar. Seminggu pun dah cukup untuk test. Kalau nak memahami lebih mendalam, buatlah sebulan. Dalam masa sebulan itu, kalau konsisten penggunaan strategi tersebut, kita boleh:

* Faham bila strategi tu 'menjadi' atau tidak
* Tahu bila dan kenapa dia gagal
* Kenal pasti waktu market sesuai atau tak sesuai guna strategi tu

Manual trading ni macam proses 'berkawan' dengan strategi kau. Kau akan faham perangai dia — cepat atau lambat, suka trending ke ranging market, dan sebagainya.

**Kenapa Ini Penting?**

Kebanyakan orang terus nak automate tanpa faham **"tingkah laku strategi"** tersebut. Bila algo gagal, mereka salahkan kod — padahal strategi tu sendiri tak sesuai dengan market condition.

## 3. Apa Itu Tingkah Laku Strategi?

Setiap strategi ada tingkah laku nya tersendiri. Sama seperti apa yang aku pernah bincangkan dalam post [Analisis Teknikal](https://luangdiri.github.io/2021/08/12/analisis-teknikal.html), setiap pair symbol juga ada personaliti nya tersendiri. Maka untuk mengenal personliti pairing tersebut haruslah kita kenalpasti tingkah laku strategi kita.

Contoh:

* Moving Average berfungsi lebih baikdalam market trending
* RSI divergence mungkin lebih berkesan di pasaran ranging
* Strategi yang 'menjadi' di BTC/USD mungkin tak sesuai untuk XRP/USD
* Timeframe 1H mungkin tak menjadi dengan timeframe 1D

**Strat behavior** maksudnya bagaimana strategi bertindak dalam **pelbagai keadaan pasaran dan pair**. Algo yang sama bila apply dekat pair yang lain, hasilnya boleh berubah total. Inilah sebab kenapa perlu kenal betul-betul perangai strategi kau sebelum automate.

## 4. Scripting (Penulisan Kod)

Bila kau dah yakin dengan strategi tu, barulah masuk fasa scripting — **convert strategi jadi kod**.

### 4.1 Bahasa Apa Nak Guna?

Ada dua laluan utama:

* **Pinescript** - TradingView
* **Python** - untuk automation/backtest guna library macam `backtrader`, `pandas-ta`, `ccxt`

Tujuan scripting ni adalah untuk:

* Automate deteksi signal
* Buat backtest
* Buat alert (TradingView)
* Trade secara automatik (live trading)

## 5. Backtest di TradingView

Backtest adalah proses **uji strategi terhadap data lama**. Ini tak menjamin kejayaan masa depan, tapi **membantu memahami prestasi strategi dalam pelbagai kondisi pasaran**; down trend, up trend, ranging. TradingView adalah platform paling mudah untuk kita nak backtest strategi kita. Cuma kena fahamkan Pinescript je lah. Kalau kau dah ada strategi dalam format lain pun boleh pakai ChatGPT untuk convert strategi tersebut kepada Pinescript.

### 5.1 Guna Alert untuk Uji Keberkesanan

Dalam TradingView, kita boleh:

* Letak alert untuk buy/sell signal
* Tengok sama ada alert tu logik atau tak bila market bergerak
* Melihat prestasi backtest
* Mengubah setting initial capital untuk backtest

### 5.2 Pentingnya Faham Kelemahan Backtest

* Backtest **tidak sama** dengan live trading
* Slippage, latency, spread, dan efek berita tak selalu terjelma dalam proses backtesting
* Tapi... tetap penting untuk kenal pasti "strat behavior" menggunakan data lampau

Backtest juga boleh memberikan keyakinan sama ada strategi tu **ada edge** atau sekadar syok sendiri.