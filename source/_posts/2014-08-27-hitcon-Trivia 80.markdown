---
layout: post
title: "HITCON Pneumotoulthami...."
date: 2014-08-27 11:17:48 +0430
comments: true
categories: 
---
Pneumotoulthamicrescopicfilicoloaganiconissis was another Trivia challange in HITCON ctf with 80 points .

gave us a big text file http://bit.ly/XRnRWi , searching title of challange in google i found this name "Pneumonoultramicroscopicsilicovolcanoconiosis"
<!--more-->
it scientific name of a disease but title and original name had differences if you find them it wolud be "the flag" but just "the flag" , we knew that flag should have HITCON{}
then admins revealed a description : 
>diff the file content and the origin english word (try to find it out)

with this clue i googled strings of text file in google and found original text , it was name of Titin chemcial substance , now we should just copmare original and moderated 
texts character by character 
with python we can read strings and push them to 2D list using zip() and compare them 

Our Code :
```python
#!/usr/bin/python
mod = 'moderated titin name'
orig = 'original titin name'
res = ''
for x, y in zip(mod,orig):
    if x != y:
        res += x
print res
```

![alt text](http://up.ashiyane.org/images/m4gyi584z244s20esqiw.png "HITCON triv2")

The flag is :
```perl 
HITCON{This flag is longestestestestestestestestestestestestestestestestestestestestestestestestestest!!!}
```

