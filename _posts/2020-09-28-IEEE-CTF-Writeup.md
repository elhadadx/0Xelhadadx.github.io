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

* exiftool * 

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

> **Flag: IEEE{CaesarAintH4rd}**

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

# []()3.warm up

![](https://i.ibb.co/Gs7wTpX/warmup.png)

> **we hava base64 hash and when we decrypt it i got another md5 hash, but we need to fix the hash as mensioned in the description of the challenge.**

![](https://i.ibb.co/RC3vzM6/base64.png)

* after analyse this hash i optain that this a hex value from **a-f** so we need to delete the char **h** and **z** to get the right md5 hash

> **Flase md5 hash: 482c811dha5d5b4bc6d497ffa98491ze38**

> **Correct md5 hash: 482c811da5d5b4bc6d497ffa98491e38**

![](https://i.ibb.co/mD1VSgB/flagmd5.png)

> **Flag: IEEE{password123}

# []()4.Brute Me

![](https://i.ibb.co/hLRVjM1/bruteme.png)

* You can download the file from [here](https://filebin.net/3z6ni0dvk2oj9rdn/flag.zip?t=55c8jhxy)

> i just cracked tha password and got the flag.

> **fcrackzip -u -v -D -p /usr/share/wordlists/rockyou.txt flag.zip**

* **Password: sainsburys**

![](https://i.ibb.co/C7kzNYk/brute-flag.png)

> **Flag: IEEE{Easy_Brute}**

* nice!, we are done from the Misc now, let's go to the Web now.






