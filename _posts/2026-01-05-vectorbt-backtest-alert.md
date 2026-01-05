---
title: 'VectorBT Backtest & Telegram Alert'
date: 2026-01-05 14:39:47
updated_at: 2026-01-05 14:39:47
tags:
- tutorial
- trading
---

*Work In Progress*

<img alt="backtesting.py" src="https://i.imgur.com/gvFn4Wn.png">

Pergi ke [Bahagian I][2]

Dari dulu lagi bercita-cita nak kasi trading strategy aku di automasi kan. Kali ni dengan bantuan LLM, akhirnya aku berjaya bina stack aku sendiri. Stack yang aku bina ni takde lah advanced sangat kalau nak dibandingkan dengan stack teknologi yang quant dari firm besar pakai, tapi bagi aku dah cukup untuk permulaan seorang trader yang baru nak berkecimpung dalam dunia algo trade.

Kalau dulu aku buat dari scratch, memang berterabur code aku. Tak ada standard, kalau nak ubah parameter kena ubah semua file. Sekarang bila dah tahu konsep sebenar, baru boleh atur dengan jelas. 

Basically nak bina stack backtest ni, langkah pertama kena ada strategy yang kita cipta atau jumpa di internet. Dalam post ni pun aku akan cerita pasal strategy MA crossover biar senang nak cerita. Aku juga ada buat alert untuk di hantar ke Telegram channel. Aku tak host dekat mana-mana cloud server pun, cuma guna Raspberry Pi untuk run script alert ni.

---
**Kandungan:**

- [Permulaan](#permulaan)
  - [Apa itu backtest?](#apa-itu-backtest)
  - [Prosedur operasi](#prosedur-operasi)
  - [Stack](#stack)
- [Trade-Offs](#trade-offs)
- [Penutup](#penutup)


---

## Permulaan

### Apa itu backtest?

Berbalik kepada cerita asal, backtesting adalah satu fasa dalam sistematik trading iaitu menguji strategi dengan menggunakan data harga di masa lampau. Kita nak uji strategi tersebut berkesan atau tidak bergantung kepada keadaan market. Market price ada beberapa fasa juga iaitu, uptrend, sideway dan downtrend. Ada juga orang sebut fasa ini sebagai accumulation, expansion dan distribution. Ikut la mana suka, konteks nya tetap sama.

Bila kita uji strategi kita tu, kita cuma melihat performance nya dengan waktu yang dah berlalu. Bererti ujian tu mungkin boleh atau tak boleh diterapkan untuk perjalanan harga di masa depan. Kata lain, past performance tak menjamin future performance. Itu sebab kita kena backtest strategi tersebut dengan data sample yang banyak.

Data sample dalam hal ini merujuk kepada data sejarah harga dari awal sesebuah pair itu hidup di market sehingga lah sekarang. Lagi banyak data sample yang kita dapat, lebih bermutu final result yang di keluarkan dari ujian backtest.

### Prosedur operasi

Untuk permulaan, kita perlu ada strategi untuk melaksanakan proses sistematik trading ni.

1. Idea strategy
2. Convert strategy jadi code
3. Integrate code dalam framework
4. Lakukan backtest
5. Run alert bot (optional)

Biasa nya prosedur yang biasa aku buat bila dah dapat sesuatu strategi adalah:

1. Backtest dulu dalam TradingView -- guna Pine Script
2. Convert Pine Script ke Python -- guna LLM
3. Integrate ke dalam backtesting.py -- backtest framework

Kenapa TradingView?

Mungkin lain orang ada cara nya tersendiri. Tapi sebagai seorang yang cheapskate macam aku, aku akan mulakan dengan benda yang free dulu. TradingView ada juga feature untuk backtest strategi menggunakan Pine Script (scripting language untuk TradingView). Cuma historical price data untuk free tier nya terhad kepada 5,000 bars sahaja (intraday timeframe). Jadi tak syok la tak dapat guna semua data yang ada. Macam aku cakap tadi, lagi banyak data kita ada, lagi bagus result backtest.

> ⚠⚠ Iklan ⚠⚠  
> Register akaun TradingView menggunakan ref link ini:  
> [https://www.tradingview.com/pricing/?share_your_love=natebroke][3]  
> Kita kan kawan :)

### Stack

Tech yang digunakan sepanjang perjalanan backtesting ni adalah:

|||
|---|---|
| **Language** | Python, Pine Script |
| **Backtest framework** | [backtesting.py][1] |
| **Alert** | Telegram |
| **LLM** | Grok, ChatGPT |
| **Server** | Raspberry Pi 3 Model B Rev 1.2, 1GB |
| **Server OS** | Debian 12 (bookworm) |

Tak ada Raspberry Pi pun takpe, benda tu untuk kita jalankan alert bot selama 24/7. Guna laptop pun boleh, cuma kena biarkan on je lah.

Untuk engine backtest, aku gunakan [backtesting.py][1] framework. Ini adalah framework untuk backtest strategy trading yang ditulis dalam Python.

Aku pilih framework ini sebab aku tengok dia *macam* lightweight, sebab feature nya tak sebanyak framework backtest lain. Mula-mula aku nak guna [Hummingbot][4], tapi learning curve dia agak curam ya. Walaupun guna LLM, aku tetap jadi bodoh dalam mengintegrasikan Hummingbot dalam sistem aku. Mungkin sebab sasaran objektif aku cuma sikit je kot; nak dapatkan backtest result. Bila dah ada banyak feature yang bloated dalam sesebuah framework tu kan otak kita jadi overwhelm.

Jadi backtesting.py ni engine nya hanya fokus kepada kerja nak backtest saja. Tak ada pun feature untuk live trade mahupun native Telegram alert. Ya, untuk alert tu aku custom sendiri. Bahagian Alert nanti aku ada cerita.


## Trade-Offs

Framework khas untuk backtesting saja. Live alert sebenarnya terlewat satu candle, tak sama dengan behavior tradingview. alert juga untuk entry/reversal entry dan bukan close position signal sebab backtesting.py tak salurkan info ni. berhari2 aku cuba hack untuk dapatkan info close trade, tapi tak dapat. kalau set finalize_trade=true, dia akan close setiap candle baru. kalau false, dia tak plot pun position yang paling latest yang masih open. closed_trades property yang null tu untuk closed trade yang dah lepas, bukan yang latest.

jadi nak yang accurate kena pakai framework khas untuk live trading, lain kata, framework yang calculate trade on tick.

## Penutup

backtesting.py made this possible without much hassle. The code stays the same for backtest and live. No drift.

It's basic, but reliable. When I want to add filters or more strategies, the structure is ready.

That's how I got a simple MA cross strategy from idea to running on a Pi. Clean, organized, and useful.

[1]: https://kernc.github.io/backtesting.py/
[2]: https://luangdiri.github.io/2021/08/21/perdagangan-algoritma-bhg-1.html
[3]: https://www.tradingview.com/pricing/?share_your_love=natebroke
[4]: https://hummingbot.org/
[5]: https://wundertrading.com/en/register?ref=wbtdf816e60
[6]: https://i.imgur.com/rhqtB4C.png
[7]: https://kernc.github.io/backtesting.py/doc/backtesting/