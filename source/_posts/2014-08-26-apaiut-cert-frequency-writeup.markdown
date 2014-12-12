---
layout: post
title: "APAIUT-CERT Frequency Writeup"
date: 2014-08-26 22:26:54 +0430
comments: true
categories: 
---
Frequency had 30 points in APAIUT-CERT CTF .

the cipher was : 

QV KZGXBWOZIXPG, I KIMAIZ KQXPMZ, ITAW SVWEV IA KIMAIZ'A KQXPMZ, BPM APQNB KQXPMZ, KIMAIZ'A KWLM WZ KIMAIZ APQNB, 

<!--more-->

QA WVM WN BPM AQUXTMAB IVL UWAB EQLMTG SVWEV MVKZGXBQWV BMKPVQYCMA. QB QA I BGXM WN ACJABQBCBQWV KQXPMZ QV EPQKP 
MIKP TMBBMZ QV BPM XTIQVBMFB QA ZMXTIKML JG I TMBBMZ AWUM NQFML VCUJMZ WN XWAQBQWVA LWEV BPM ITXPIJMB. NWZ MFIUXTM,
EQBP I TMNB APQNB WN 3, L EWCTL JM ZMXTIKML JG I, M EWCTL JMKWUM J, IVL AW WV. BPM UMBPWL QA VIUML INBMZ RCTQCA KIMAIZ,
EPW CAML QB QV PQA XZQDIBM KWZZMAXWVLMVKM. NTIO: BP1AEIAMIAG

without any analyzing we can understand from name of challange it's something between Vignere , Caesar , Autokey
Autokey is substitution cipher wich had a key for changing place of each Character and also we can solve it as Cryptogram and Puzzle with Code breakers 
so i used SCB Solver or you can use this online script named quipquip at http://www.quipqiup.com/ for this Cipher

secret key:
IJKLMNOPQRSTUVWXYZABCDEFGH (means the changes of alphabet order in english)

decoded message:

in cryptography, a caesar cipher, also known as caesar's cipher, the shift cipher, caesar's code or caesar shift,
is one of the simplest and most widely known encryption techniques. it is a type of substitution cipher in which 
each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet. for example, 
with a left shift of 3, d would be replaced by a, e would become b, and so on. the method is named after julius caesar,
who used it in his private correspondence. flag: th1swaseasy

at the end of decoded cipher you can find the flag :

```perl
th1swaseasy
```

30 points ;)



