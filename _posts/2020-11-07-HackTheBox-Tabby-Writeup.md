---
title: HackTheBox Tabby Writeup
published: true
---

![](https://i.ibb.co/mBPb1hs/logo.png)

# []()Methodology

* nmap scan
* LFI Lead to read tomcat user credentials
* exploitation by Metasploit
* crack zip file fcrackzip 
* own user
* Privilege Escalation

# []()Nmap Scan

> as always, i’ll do nmap scan to find out which services running in this machine.

* 8080 for apache tomcat

* 22 for ssh

* from nmap scan i know that there is a apache tomcat server.

```ruby

# Nmap 7.80 scan initiated Sun Jun 21 05:43:17 2020 as: nmap -sC -sV -oN scan.txt 10.10.10.194
Nmap scan report for 10.10.10.194
Host is up (0.13s latency).
Not shown: 997 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Mega Hosting
8080/tcp open  http    Apache Tomcat
|_http-open-proxy: Proxy might be redirecting requests
|_http-title: Apache Tomcat
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

> let's check the web page now.

![](https://i.ibb.co/b5RHnQj/webpage-mega.png)

> after looking at the source code i found the way to exploit LFI, let's see how.

![](https://i.ibb.co/BNg7ZrK/lfi-param.png)

# []() LFI Part

* from the source code i've found the url: http://megahosting.htb/news.php?file=

> the file parameter was vulnerable to LFI, let's check it.

![](https://i.ibb.co/qNY5mWv/lfi-done.png)

* now we have lfi, let's check the port 8080 now.

![](https://i.ibb.co/Hp3ND9s/port-8080.png)

> from this page we know that the users data we will find it at **tomcat9/tomcat-users.xml**, let's use lfi now.

* from Apache tomcat [Docs](http://tomcat.apache.org/tomcat-8.5-doc/manager-howto.html) i know that the file will be in **/usr/share/tomcat9/etc/tomcat-users.xml**

* if you are a linux user you will know that without any blogs because any program data will be found at **/usr/share**

> here is the credentials for apache tomcat server.

![](https://i.ibb.co/xfR6Rjq/user-data.png)

> now we have the credentials, let's use metasploit now to exploit this service.

