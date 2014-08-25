---
layout: post
title: "APAIUT-CERT colors writeup"
date: 2014-08-25 20:33:17 +0430
comments: true
categories: 
---

Hi

"colors" was third crypto challange in APAIUT-CERT CTF which had 40 points .
they gave us this file with 11 circles .


![alt text](http://up.ashiyane.org/images/29g2rhia2m2dt8jq83mr.png "crypto 3")

<!--more-->

I had nothing in my mind till CTF admins gave a hint 
>Look at colors codes.

then i tried to extract color codes with Photoshop or GIMP
our codes had lot of Zero's
trying to find a way to decode that codes they gave another hints 
>Remove Extra Zero's.

finally it was our codes 


```perl
63 84 189 238 45 112 175 140 56 189 98
```


i tested ascii-table , Hex , Decimal , and .... or decodeing these codes but nothing changed because these codes had'nt special base aand Radix.

i went to mathematics :) so decided to find Greatest common divisor of all numbers
first 2 by 2 tested each number till 45 no divisor found with other numbers but other numbers had divisor , then deleted 45 and test for divisor of all numbers to gether 
the result was "7" ...

divided all numbers to 7 expect 45 
this was the result 

```perl
9 12 24 34 ? 16 25 20 8 24 14
```

these numbers had a base and radix between 0-36 
after testing lot of classic ciphers i found Letter Number cipher
but with a small diffrence letter number is a-z similar to 0-27 
but our codes were like this a-z+0-9 similar to 1-26+27-36
in perl language it is like this : 

```perl
@char{("a".."z")} = (1..26);
```
 
```perl
@num{("a".."z")} = (27..36);
```


With admin hints :) we could find the flag, i cant remember exactly but it should be the flag 

The flag was :

```perl
il073pyth0n
```
  
  Cheers <3