---
title: "Mathematics of RSA"
date: 2012-10-17T00:00
tags:
- science
---

_First posted on 17/10/2012_

Berikut adalah contoh matematik ringkas tentang RSA encryption dan decryption.

1. Alice pilih dua prime numbers yang besar, namanya p dan q. Nombor tu kena lah besar utk security yang mantap. Tapi untuk example ni guna yang kecik dulu, p = 17 dan q = 11. p dan q Alice pegang ni hendaklah di rahsia kan.
2. Nombor p dan q tersebut haruslah di darabkan untuk hasilkan satu nombor yang baru, iaitu N. Dalam kes ni, N = 187. Sekarang Alice kena pilih satu lagi nombor namanya e. Katakan lah dia pilih e = 7.
3. Sekarang Alice boleh kasi nombor N dan e dekat orang awam. Nombor nombor ini lah dinamakan Public-Key.
4. Untuk proses encryption, message perlu di convert ke nombor, M. Nombor ini adalah dalam bentuk ASCII bits (1's and 0's). Bits ni pulak boleh diterjemahkan dalam bentuk decimal nombor.
5. Equation untuk encryption: ![tex1][tex1]
6. So, Bob nak hantar Alice satu message, contoh nya satu huruf 'X'. Dalam bits, X = 1011000, bersamaan dengan 88 dalam decimal. Jadi, M = 88.
7. Untuk encrypt X, Bob kena cari public-key Alice iaitu N = 187 dan e = 7.  
Dengan M = 88, formulanya ![tex2][tex2]
2. Here's how to solve this. Boleh jugak guna calculator and stuff tapi ada jalan alternatif. Kita tahu yang 7 = 4 + 2 + 1.  
    ![tex3][tex3]  
    ![tex4][tex4]  
    ![tex5][tex5]  
    ![tex6][tex6]  
    ![tex7][tex7]  
Bob boleh hantar encrypted message, C = 11 kepada Alice.
1. Katakanlah ada seorang penjenayah ni nak eavesdrop message Bob tadi. Nama dia Eve. Eve dapat tap data dan received message Bob. Tapi dia tak boleh nak decipher message tu sebab exponentials dalam modular aritmetics (mod) adalah one-way function. Jadi susah nak reverse engineer nombor tersebut utk dapat kan plain text. Hanya Alice saja yang boleh encipher kat sini since dia ada value p dan q tadi. Dia boleh encipher untuk dapatkan d, decryption key aka Private-Key.  
Formula untuk decryption ialah  
    ![tex8][tex8]  
    ![tex9][tex9]  
    ![tex10][tex10]  
    ![tex11][tex11]  
Ada satu teknik dipanggil Euclid's algorithm boleh selesaikan d dengan pantas.
1. Untuk decrypt message, Alice guna formula ini:  
    ![tex12][tex12]  
    ![tex13][tex13]  
    ![tex14][tex14]  
    ![tex15][tex15]  
    ![tex16][tex16]

Benda benda ni semua dipanggil RSA encryption. Direkacipta oleh abang Rivest, Shamir dan Adleman. RSA ialah one-way function, which is orang yang ada information tertentu saja yang boleh decrypt. Information tu ialah p and q tadi lah.   

---

Sauce: Appendix E of '[The Code Book][codebook] by Simon Singh'


[tex1]: https://latex.codecogs.com/gif.latex?C%20%3D%20M%5Ee%20%28mod%20N%29
[tex2]: https://latex.codecogs.com/gif.latex?C%20%3D%2088%5E7%20%28mod%20187%29
[tex3]: https://latex.codecogs.com/gif.latex?88%5E7%20%28mod%20187%29%20%3D%20%5B88%5E4%20%28mod%20187%29%20*%2088%5E2%20%28mod%20187%29%20*%2088%5E1%20%28mod%20187%29%5D%20%28mod%20187%29
[tex4]: https://latex.codecogs.com/gif.latex?88%5E1%20%3D%2088%20%3D%2088%20%28mod%20187%29
[tex5]: https://latex.codecogs.com/gif.latex?88%5E2%20%3D%207%2C744%20%3D%2077%20%28mod%20187%29
[tex6]: https://latex.codecogs.com/gif.latex?88%5E4%20%3D%2059%2C969%2C536%20%3D%20132%20%28mod%20187%29
[tex7]: https://latex.codecogs.com/gif.latex?88%5E7%20%3D%2088%5E1%20*%2088%5E2%20*%2088%5E4%20%3D%2088%20*%2077%20*%20132%20%3D%20894%2C432%20%3D%2011%20%28mod%20187%29
[tex8]: https://latex.codecogs.com/gif.latex?e%20*%20d%20%3D%201%20%28mod%20%28p%20-%201%29%20*%20%28q%20-%201%29%29
[tex9]: https://latex.codecogs.com/gif.latex?7%20*%20d%20%3D%201%20%28mod%2016%20*%2010%29
[tex10]: https://latex.codecogs.com/gif.latex?7%20*%20d%20%3D%201%20%28mod%20160%29
[tex11]: https://latex.codecogs.com/gif.latex?d%20%3D%2023
[tex12]: https://latex.codecogs.com/gif.latex?M%20%3D%20C%5Ed%20%28mod%20187%29
[tex13]: https://latex.codecogs.com/gif.latex?M%20%3D%2011%5E23%20%28mod%20187%29
[tex14]: https://latex.codecogs.com/gif.latex?M%20%3D%20%5B11%5E1%20%28mod%20187%29%20*%2011%5E2%20%28mod%20187%29%20*%2011%5E4%20%28mod%20187%29%20*%2011%5E16%20%28mod%20187%29%5D%20%28mod%20187%29
[tex15]: https://latex.codecogs.com/gif.latex?M%20%3D%2011%20*%20121%20*%2055%20*%20154%20%28mod%20187%29
[tex16]: https://latex.codecogs.com/gif.latex?M%20%3D%2088%20%3D%20X
[codebook]: https://www.amazon.com/Code-Book-Science-Secrecy-Cryptography/dp/0385495323
