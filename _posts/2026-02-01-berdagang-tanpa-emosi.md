---
title: 'Berdagang Tanpa Emosi'
date: 2026-02-01 21:09:05
updated_at: 2026-02-01 21:09:09
tags:
- tutorial
- trading
---

*WIP*

*[Bahagian I][1] - Perdagangan Algoritma*  
*[Bahagian II][2] - Vectorbt & Backtest Alert*

Dalam [post][2] yang terakhir kita sudah teroka bagaimana Vectorbt sebagai backtest engine berfungsi dan menggunakan signal yang terhasil untuk dihantar ke Telegram sebagai alert berkala.

Dalam post kali ini, aku akan membincangkan tentang live trading yang mengintegrasikan platform seperti Binance dan Hyperliquid untuk melaksanakan sesebuah trade.

Seperti sedia maklum, signal yang terhasil dari Vectorbt boleh digunakan untuk menghantar alert ke Telegram. Dalam masa yang sama, kita juga boleh inject function baru iaitu memulakan trade di exchange pilihan. Dalam post ini aku akan cerita macam mana nak membuka trade di Binance menggunakan package Python iaitu [CCXT][ccxt].

- [Integrasi bersama alert](#integrasi-bersama-alert)


## Integrasi bersama alert

Secara teori nya, 


[1]: https://luangdiri.github.io/2021/08/21/perdagangan-algoritma-bhg-1.html
[2]: https://luangdiri.github.io/2026/01/04/vectorbt-backtest-alert.html
[ccxt]: https://github.com/ccxt/ccxt