---
title: IEEE CTF Challenges Writeup 
published: true
---

![logo](https://i.ibb.co/YPR3X9G/logo.png)

> **hello friends, this is My writeup for IEEE CTF Misc && RE && Forensics writeup**.

# []() Foreniscs Challenges Writeup

![easy](https://i.ibb.co/MsX3xL7/easy.png)

* this challenge was very easy, i checked it with binwalk and i found that there is hidden photo's, then i extracted them and with exiftool i got the flag.

> You can download the challenge from here [EASY](https://filebin.net/3z6ni0dvk2oj9rdn/EASY.dd?t=begirmoz).

* let's start with **binwalk**

> **binwalk EASY.dd**

![](https://i.ibb.co/XS7Hrwm/easy-binwalk.png)

* now we have 4 photos in this dd image, let's extract them now.

> **binwalk --dd='.*' EASY.dd**

* i got this file **_EASY.dd.extracted** let's open it now.

![](https://i.ibb.co/1mvcHbR/extracted.png)

> now we need to check all of them with **exiftool**

> **exiftool * **

![](https://i.ibb.co/hmvppZq/comment.png)

* there is a Comment!

> **Comment                         : VRRR{Gu!f_vf_Gur_sy@t_T00q-W0o}**

* looks like a flag!, yes this is our flag but it's encrypted with **Rot** and you can decrypt it with this [website](https://www.dcode.fr/rot-cipher)

![](https://i.ibb.co/0tmLsbn/flag.png)

> **Flag: IEEE{THSISTHEFLGGDJB}**






