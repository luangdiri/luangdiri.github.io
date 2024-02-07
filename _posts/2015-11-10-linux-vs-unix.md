---
title: 'Linux vs UNIX'
date: 2015-11-10 07:20:30 +08:00
tags:
---

tl;dr

**Linux** ialah FOSS
**UNIX** ialah proprietary.

## Sejarah

*Directly translated from [Albion](http://www.albion.com/security/intro-2.html)*

Untuk menerangkan Unix, lagi bagus kalau kita tengok sejarah nya. Pada tahun 1969, bermulanya kerja pembinaan UNIX oleh Ken Thompson, Dennis Ritchie dan lain lain menggunakan minicomputer PDP-7 di AT&T Bell Labs. Selama 10 tahun, pembinaan UNIX telah melangkau beberapa versi. Versi 4 (1974) telah di tulis dalam C -- juga satu milestone besar untuk portability antara operating system yang lain. Versi 6 (1975) adalah yang pertama dibina diluar Bell Labs -- versi pertama yang di bina di University of California Berkeley.

Tahun 1980, Bell Labs meneruskan pembinaan UNIX dengan mengeluarkan System V (1983) dan System V, Release 4 (SVR4) (1989). Dalam pada tu, programmers di University of California dapat explore source code yang AT&T released, menyebabkan banyak thesis di published. The Berkeley Standard Distribution (BSD) menjadi satu variasi besar untuk UNIX. Ia telah digunakan oleh sistem universiti dan sektor korporat bermula dengan BSD 4.2 pada tahun 1984. Beberapa daripada feature extra ni telah pun digunakan di dalam SVR4.

Masuk 1990, lesen source code AT&T telah membuka banyak lagi market untuk variasi UNIX oleh vendor yang berbeza. AT&T jual bisnes UNIX ni kepada Novell pada tahun 1993 dan Novell pulak jual kepada Santa Cruz Operation dua tahun kemudian. Dalam masa tu, trademark UNIX telah pun di ambil alih oleh konsortium X/Open, setelah beberapa ketika membentuk The Open Group.

Dikala konflik trademark UNIX yang berlarutan, beberapa pembinaan yang masih berjalan bermula untuk membuahkan hasil. Secara tradisinya, untuk membuat sistem BSD ni berjalan, kita perlukan lesen source code dari AT&T. Tapi dalam tahun 1990, Berkeley hackers dapat mengeluarkan most of the proprietary source code dalam sistem UNIX dan digantikan dengan yang open source. William dan Lynne Jolitz adalah beberapa programmer masa tu. Menggodam distribution BSD iaitu Net, akhirnya dapat mengeluarkan 386BSD versi 0.1 pada tahun 1992. Ada 3 distribution yang menggunakan "free source" BSD ni iaitu, NetBSD, FreeBSD dan OpenBSD, semuanya based on BSD 4.4.

BSD bukan lah attempt pertama untuk UNIX "bebas" (lol). Pada tahun 1984, Richard Stallman memulakan kerja untuk klon free UNIX dan diberi nama **GNU** (GNU's NOT UNIX). Permulaan tahun 1990, projek GNU dapat mengejar beberapa programming milestone, termasuk GNU C library dan **Bourne Again SHell** (bash). Sistem yang penuh dapat disiapkan secara am nya, kecuali satu benda: kernel yang berfungsi.

Memperkenalkan Linus Torvalds, seorang pelajar University of Helsinki di Finland. Linus melihat satu sistem UNIX kecil yang dinamakan Minix dan rasa dia boleh buat lagi bagus. Pada tahun 1991, dia mengeluarkan source code untuk kernel freeware dipanggil "Linux" -- satu kombinasi nama dia dan Minux. Pada tahun 1994, Linus dan beberapa hackers mengeluarkan versi 1.0 Linux. Linus dan kawan kawan dia ada free kernal; Stallman dan kawan kawan dia pulak ada free UNIX klon sistem: Maka terhasil lah gabungan kernel Linux dan klon UNIX yang sekarang dipanggil Linux -- tapi Stallman lebih prefer ia dipanggil "GNU/Linux system". Ada beberapa perbezaan antara GNU/Linux distribution: beberapa daripada nya terdapat dalam sistem komersial seperti Red Hat, Caldera Systems, dan SUSE; yang lain, seperti Debian GNU/Linux, adalah lebih dekat dengan konsep free software.

Pengembangan sayap Linux, yang sekarang di versi 2.2, telah pun membuat satu fenomena. Linux dapat dijalankan dalam architecture chip yang berbeza dan boleh di support oleh vendor vendor UNIX seperti HP, Silicon Graphics dan Sun Microsystems, PC vendors macam Compaq dan Dell, dan software vendor Oracle dan IBM. Ironinya sekarang adalah Microsoft, yang dah acknowledge pasal benda ni tapi takde respon untuk buat sistemnya open-source.

Microsoft telah pun mengeluarkan Windows NT (Windows 2000). Pada tahun 1990, banyak vendor telah meninggalkan platform server UNIX, untuk digantikan dengan Windows NT. Silicon Graphics, contohnya, membuat keputusan yang Intel hardware dan NT adalah platform graphic untuk masa hadapan.

Fenomena vendor vendor lompat bandwagon dari UNIX ke Linux ke Microsoft ni semua membuatkan kita tertanya tanya balik pasal "Apakah itu UNIX?". Sebagai satu base software untuk Internet yang kita pakai sekarang ni, UNIX adalah satu teknologi yang signifikan untuk tamadun manusia 20-an.

## Apa itu Unix-like

Also known as ***nix** or **UN*X**

Bila kita cakap sistem tu *Unix-like*, sistem tu sebenarnya ada serpihan serpihan Unix code di dalam nya. FreeBSD contohnya adalah Unix sistem tapi part proprietary nya digantikan dengan open source.

## Commands

Linux commands datang daripada GNU. Dengan itu, command GNU direka untuk replace, sekaligus compatible dengan UNIX command. Jadi, experience pakai dua dua system ni boleh dikatakan sama. Sama ada pakai Unix-like OS atau UNIX, it should be almost the same.

Kalau nak lagi pure, cuba pakai freeBSD. One of the direct descendant of UNIX.

## GNU/XNU

Just curious. GNU as we all know, GNU Is Not UNIX. **XNU** pulak adalah kernel bagi Darwin OS, iOS, OS X developed by Apple. Acronym untuk **X is Not UNIX**. Pada asalnya, XNU direka oleh NeXT untuk NeXTSTEP OS yang lepas tu dibeli oleh Apple.

## UNIX/Unix/unix

> The name "Unix" was intended as a pun on the name Multics and was written "Unics" at first, for UNiplexed Information and Computing System. Both "Unix" and "UNIX" are in wide use today. At one point, Dennis Ritchie tried to promulgate the lower-case version, since "UNIX" isn't an acronym. In deference to the trademark, this text uses "UNIX."
> 
> -- Excerpt from Unix System Security Tools by Seth T. Ross

#### Source

- [http://www.makeuseof.com/tag/linux-vs-unix-crucial-differences-matter-linux-professionals/](http://www.makeuseof.com/tag/linux-vs-unix-crucial-differences-matter-linux-professionals/)
- [http://www.diffen.com/difference/Linux_vs_Unix](http://www.diffen.com/difference/Linux_vs_Unix)
- [http://www.unix.org/what_is_unix/history_timeline.html](http://www.unix.org/what_is_unix/history_timeline.html)
- [http://www.levenez.com/unix/](http://www.levenez.com/unix/)
- [https://en.wikipedia.org/wiki/History_of_Unix](https://en.wikipedia.org/wiki/History_of_Unix)
- [https://upload.wikimedia.org/wikipedia/commons/7/77/Unix_history-simple.svg](https://upload.wikimedia.org/wikipedia/commons/7/77/Unix_history-simple.svg)
- [http://www.albion.com/security/intro-2.html](http://www.albion.com/security/intro-2.html)
