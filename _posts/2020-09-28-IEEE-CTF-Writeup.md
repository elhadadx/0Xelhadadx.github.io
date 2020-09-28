---
title: IEEE CTF Challenges Writeup 
published: true
---

![logo](https://i.ibb.co/YPR3X9G/logo.png)

> **hello friends, this is My writeup for IEEE CTF Misc && RE && Forensics writeup**.

# []() Foreniscs

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

> **exiftool ***

![](https://i.ibb.co/hmvppZq/comment.png)

* there is a Comment!

> **Comment                         : VRRR{Gu!f_vf_Gur_sy@t_T00q-W0o}**

* looks like a flag!, yes this is our flag but it's encrypted with **Rot** and you can decrypt it with this [website](https://www.dcode.fr/rot-cipher)

![](https://i.ibb.co/0tmLsbn/flag.png)

> **Flag: IEEE{THSISTHEFLGGDJB}**

# []() Misc Challenges

# []()1.Caesar salad

![](https://i.ibb.co/XtnxwQR/easy.png)

> **from the challenge name i thought that this a Caesat Cipher but it was, Rail Fence Cipher, i tried all the Ciphers in this website til i got the flag.**

* use this [website](https://cryptii.com/pipes/caesar-cipher) to decrypt the string.

![](https://i.ibb.co/fqxbJpq/flag1.png)

> **Flag: IEEE{CaesarAintH4rd}

# []()2.Uns3cure

![](https://i.ibb.co/vYPJgGX/unsecure.png)

> we got a pcap file and i opened it with wireshark and it's so easy to find the plaintext password, you can download the file from [here](https://filebin.net/3z6ni0dvk2oj9rdn/unsecure.pcap?t=3muyooly)

* open wireshark and look carefully with me

![](https://i.ibb.co/x5QrKSB/packet.png)

> **in this packet we can see that there is someone tell anotherone to login with ssh to make something.**

![](https://i.ibb.co/gghRpbQ/flag.png)

> **in this packet we found a plaintext with the same name of the challenge and we can confirm that this is the right password from the next packet.**

![](https://i.ibb.co/9NhBs0Y/loggedin.png)

* Login successful!

> **Flag: IEEE{o_Uns3cure}**







