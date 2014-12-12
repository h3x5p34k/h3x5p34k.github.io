---
layout: post
title: "Steganography Unknown CTF"
date: 2014-08-31 14:03:35 +0430
comments: true
categories: 
---
 Hi guys ,
 This post is about Steganography challange of hicking sites , because challange is live now i changed its name to unknown ctf
 
 ![alt text](http://up.ashiyane.org/images/jvsrdh8l9xlbd53n8q.png "steg unknown")
 <!--more-->
 
 in picure you can and understand that , you have to read red pixels , but how :
 
```python
import Image
im = Image.open('stego9.png')
(h, w) = im.size
img = Image.new("RGB", (h,w), "white")
for x in range(h):
 for y in range(w):
  rgb_im = im.convert('RGB')
  r, g, b = rgb_im.getpixel((x, y))
  if (r,g,b) == (255,0,0) :
   data = x,y
   img.putpixel((x,y),(255,0,0))
img.save("flag.png")
```
Flag.png : 

![alt text](http://up.ashiyane.org/images/3050pypz0lmybvnibfj.png "steg unknown")

you can see "PASSWORD" but cant read password ..
using Stegsolve --> Image Combiner --> Vertcial Mode
Solved.png :

![alt text](http://up.ashiyane.org/images/ri56hkcwih8biry5ams.png "steg unknown")

The flag is : 
```perl
2#4%6&x!
```
Good Luck

