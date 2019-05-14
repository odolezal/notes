**Jak jsem hrál Try2Hack.me** 
==========
Web hry: <https://www.try2hack.me>
> Cílem hry je řešení rozličných úkolů z oblasti počítačové bezpečnosti (hackingu), přičemž je kladen důraz na realistický zážitek. Nejde o uměle vytvořené úlohy, nýbrž scénáře reálných útoků v rámci testovacího prostředí našeho serveru. Setkat se můžete s úkoly zaměřenými na webový hacking, infrastrukturu, reverzní inženýrství, cracking nebo kryptoanalýzu.

Hrano pod nickem: ```Ondrashack``` od 16.4.2019. 

**Stav k 7.5.2019**: vyreseno 6/15 uloh.

Kontakt: <https://www.odolezal.cz>

Uvod
---
 * **UPOZORNENI! Nasledujici radky vyzrazuji postup a/nebo rovnou heslo k jednotlivym ukolum. Pokud si nechcete zkazit zazitek ze hry, prestante cist prave nyni.**

 * Ukoly vetsinou maji vice zpusobu reseni. Moje motivace byla naucit se neco noveho a hlavne se pobavit. Nektere moje postupy se vam mozna budou zdat prilis komplikovane a pristupy k problemu usmevne najivni, ale dosel jsem k nim vlastni cestou a proto si jich cenim vice, nez kdyz by mi je nekdo poradil.

 * Reseni je uverejneno v dobe, kdy jiz je znam vytez souteze. 

 * Psano primo na serveru a anglickym prostredim, takze chybi diakritika. Psano jako denik/poznamky, korekce neni :)

Ukol c. 1 [vyreseno]
---
Hledame skrytou subdomenu k domene try2hack.me. Prvni ukol naznacuje, ze by to nemuselo byt nic hardcore.
V zadani je primo napsano, ze neni nutny bruteforce postup. Prvni logicka vec, je podivat se, na jaka jmena je vystaven HTTPS certifikat.
Moznosti je cela rada, napr. podivat se na certifikat primo v prohlizeci a nebo pres CT logy, napr <https://crt.sh/?id=1334079401>

Tak jako tak objevime:
```
X509v3 Subject Alternative Name: 
DNS:secretsubdom.try2hack.me
```

Otevreme ```secretsubdom.try2hack.me``` v prohlizeci a pokud nedojde k presmerovani na homepage, tak vime, ze jsme spravne.

```
Password: secretsubdom 
```

Ukol c. 2 [zatim nevyreseno]
---
Mame zadany login root a jeho hash hesla:
```$6$VQoztKJH$0aL8rygMd8gfX7m8cTRWOn4pqQ6bA/jkPyQSnzU0g10E0UiMQjIijs/66vflY7cMrGSKmmiBWE7r8oNCDQc3D/```

Ukol je jasny. Najit heslo, ktere se shoduje s heslem.
Nejprve bude nutne zjistit o jaky typ hashe se jedna. 
Podle ```$6``` by to znamenalo SHA512, podle dalsiho znaku ```$``` je zde pritomna sul. 

Kali ma nastroj hashid (<https://miloserdov.org/?p=1254>)

```
root@kali:~# hashid -m '$6$VQoztKJH$0aL8rygMd8gfX7m8cTRWOn4pqQ6bA/jkPyQSnzU0g10E0UiMQjIijs/66vflY7cMrGSKmmiBWE7r8oNCDQc3D/'

Analyzing '$6$VQoztKJH$0aL8rygMd8gfX7m8cTRWOn4pqQ6bA/jkPyQSnzU0g10E0UiMQjIijs/66vflY7cMrGSKmmiBWE7r8oNCDQc3D/'
[+] SHA-512 Crypt [Hashcat Mode: 1800]
```

Ktery to jasne potvrdi. Tak, co dal? Do broteforce se mi moc nechce, tak treba pujde utok slovnikem:
```
root@kali:~# hashcat --force -m 1800 -a 0 -o found1.txt --remove crack1.hash /usr/share/wordlists/10000_common_passwords
```

A vysledek (vykon na VM):
```
Session..........: hashcat                       
Status...........: Exhausted
Hash.Type........: sha512crypt $6$, SHA512 (Unix)
Hash.Target......: $6$VQoztKJH$0aL8rygMd8gfX7m8cTRWOn4pqQ6bA/jkPyQSnzU...DQc3D/
Time.Started.....: Wed Apr 17 10:03:41 2019 (1 min, 21 secs)
Time.Estimated...: Wed Apr 17 10:05:02 2019 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/10000_common_passwords)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:      123 H/s (0.94ms) @ Accel:64 Loops:32 Thr:1 Vec:4
Recovered........: 0/1 (0.00%) Digests, 0/1 (0.00%) Salts
Progress.........: 10000/10000 (100.00%)
Rejected.........: 0/10000 (0.00%)
Restore.Point....: 10000/10000 (100.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4992-5000
Candidates.#1....: booper -> eyphed

Started: Wed Apr 17 10:03:24 2019
Stopped: Wed Apr 17 10:05:03 2019
```
A vysledek zadny. Zkusime jiny, vetsi wordlist:
```
Session..........: hashcat
Status...........: Running
Hash.Type........: sha512crypt $6$, SHA512 (Unix)
Hash.Target......: $6$VQoztKJH$0aL8rygMd8gfX7m8cTRWOn4pqQ6bA/jkPyQSnzU...DQc3D/
Time.Started.....: Wed Apr 17 10:20:45 2019 (5 mins, 14 secs)
Time.Estimated...: Fri Apr 19 14:17:46 2019 (2 days, 3 hours)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       77 H/s (3.21ms) @ Accel:64 Loops:16 Thr:1 Vec:4
Recovered........: 0/1 (0.00%) Digests, 0/1 (0.00%) Salts
Progress.........: 24064/14344385 (0.17%)
Rejected.........: 0/24064 (0.00%)
Restore.Point....: 24064/14344385 (0.17%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:768-784
Candidates.#1....: hotmom -> 231992
```
Uf! Odhad hashcat-u na dva dny na nic moc VM. Necham to nejakou dobu bezet...

```
Password: ????
```

Ukol c. 3 [vyreseno]
---
URL <https://try2hack.me/AdminPanel.php> je dostupna jen z Jizni Afriky.

Jednoduse spoofnout zdrojovou IP adresu nejde, protoze TCP navazuje handshake a SYN zpatky by sel na spoofnutou adresu, tzn. spojeni se nenavaze.

Existuji ruzne online sluzby kde zadate URL a vyberete zemi ze ktere chcete web jako by prohlizet. Bohuzel, jsem nenasel web ktery by nabizel JA.

Nabizi se pouzit nejakou VPN s endpoitem v JA. To mi prijde jako moc slozite a nechce se mi instalovat klient.
Nakonec volim stare dobre HTTP/HTTPS proxy. Najit proxy server v JA neni problem (napr. <https://www.proxynova.com/proxy-server-list/country-za/>),
ale ne vsechny, ktere se tvari jako funkcni, skutecne funguji. Nicmene asi 5. server nakonec presmeruje HTTPS pozadavek (ve Firefoxu) spravne a heslo je na svete.

```
Password: Wi3ft0Wpizh8cV
```

Ukol c. 5 [zatim nevyreseno]
---
Uz podle zdani je jasne odkud vitr vane, budeme utocit na slabou implementaci sifrovani v protokolu MS-CHAPv2.
Problem je teoreticky popsan napr. zde: https://pentest.blog/attacking-wpa-enterprise-wireless-network/

Nemame primo hash hesla, ale to neni nutne protoze autori ukolu nam poskytli rovnou odchycenou dvojci challenge/response.
Hledam vhodny nastoj v Kali a po chvilce googleni nalezam... jmenuje se "asleap" (https://tools.kali.org/wireless-attacks/asleap)

Procitam  nejake info (napr.):
* <https://depthsecurity.com/blog/when-802-1x-peap-eap-ttls-is-worse-than-no-wireless-security>
* <https://www.hackingloops.com/using-asleap-on-kali-linux/>
* <https://forums.hak5.org/topic/14755-episode-6x12/>

a zkousim slovnikovi utok se znamim rockyou.txt:

```
root@kali:~# asleap -C 94:0f:90:ec:96:ce:32:ec -R f0:8f:68:d2:29:94:da:62:be:c3:6e:26:b0:b1:1d:81:d9:01:24:73:5d:dd:ba:60 -W /usr/share/wordlists/skullsecurity/rockyou.txt 
asleap 2.2 - actively recover LEAP/PPTP passwords. <jwright@hasborg.com>
Using wordlist mode with "/usr/share/wordlists/skullsecurity/rockyou.txt".
  hash bytes:        a0b8
        Could not find a matching NT hash.  Try expanding your password list.
  I've given up.  Sorry it didn't work out.
```
...a nic! Tak bud mam nevhodny wordlist a nebo neco delam spatne... no, zkusime na zname (jine) dvojci challenge/response:

```
root@kali:~# asleap -C 58:16:d5:ac:4b:dc:e4:0f -R 50:ae:a3:0a:10:9e:28:f9:33:1b:44:b1:3d:9e:20:91:85:e8:2e:c3:c5:4c:00:23 -W /usr/share/wordlists/skullsecurity/rockyou.txt 
asleap 2.2 - actively recover LEAP/PPTP passwords. <jwright@hasborg.com>
Using wordlist mode with "/usr/share/wordlists/skullsecurity/rockyou.txt".
  hash bytes:        586c
  NT hash:           8846f7eaee8fb117ad06bdd830b7586c
  password:          password
```
Ihned dostavame dopocitany NT hash a k němu heslo, nalezene ve slovniku.
Takze, slovnikovy utok se neosvedcil, bude nutne nasadit hrubou silu pomoci hashcatu.
Upravime zadani na format pro hashcat (viz napr. https://github.com/NotMedic/NetNTLMtoSilverTicket)

Hledany format hashe je nasledujici:
```
novakp::::f08f68d22994da62bec36e26b0b11d81d90124735dddba60:940f90ec96ce32ec
```

Spustime hashcat:
```
# hashcat -m 5500 -a 3 ntlm.hash

A crackujeme:
Session..........: hashcat
Status...........: Running
Hash.Name........: NetNTLMv1 / NetNTLMv1+ESS
Hash.Target......: novakp::::f08f68d22994da62bec36e26b0b11d81d90124735...ce32ec
Time.Started.....: Tue Apr 30 09:19:44 2019 (38 mins, 12 secs)
Time.Estimated...: Tue Apr 30 12:25:48 2019 (2 hours, 27 mins)
Guess.Mask.......: ?1?2?2?2?2?2?2?3 [8]
Guess.Charset....: -1 ?l?d?u, -2 ?l?d, -3 ?l?d*!$@_, -4 Undefined
Guess.Queue......: 8/15 (53.33%)
Speed.#1.........:   484.9 MH/s (4.31ms) @ Accel:1024 Loops:256 Thr:1 Vec:8
Recovered........: 0/1 (0.00%) Digests, 0/1 (0.00%) Salts
Progress.........: 1230895054848/5533380698112 (22.24%)
Rejected.........: 0/1230895054848 (0.00%)
Restore.Point....: 15302656/68864256 (22.22%)
Restore.Sub.#1...: Salt:0 Amplifier:79104-79232 Iteration:0-128
Candidates.#1....: W4q8eqab -> Y5qz2wim
```

...cekame na vysledek..

... po 6 dnech (na 16 CPU):

```
Session..........: hashcat
Status...........: Running
Hash.Name........: NetNTLMv1 / NetNTLMv1+ESS
Hash.Target......: novakp::::f08f68d22994da62bec36e26b0b11d81d90124735...ce32ec
Time.Started.....: Mon May  6 06:46:14 2019 (1 min, 34 secs)
Time.Estimated...: Fri Nov  1 07:10:49 2019 (179 days, 0 hours)
Guess.Mask.......: ?1?2?2?2?2?2?2?3?3?3 [10]
Guess.Charset....: -1 ?l?d?u, -2 ?l?d, -3 ?l?d*!$@_, -4 Undefined
Guess.Queue......: 10/15 (66.67%)
Speed.#1.........:   601.4 MH/s (6.76ms) @ Accel:1024 Loops:64 Thr:1 Vec:8
Recovered........: 0/1 (0.00%) Digests, 0/1 (0.00%) Salts
Progress.........: 55384735744/9301612953526272 (0.00%)
Rejected.........: 0/55384735744 (0.00%)
Restore.Point....: 688128/115760814336 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:5632-5888 Iteration:0-256
Candidates.#1....: Ipi5z5bere -> Qze6enhane
```
LOL!

```
Password: ????
```

Ukol c. 6 [vyreseno]
---
Opet hledame skryte subdomeny, podobne jako v ukolu c. 1. 
Na prvni pohled se to zda celkem jednoduche: hledame subdomenu, ktera obsahuje presne 3 alfanumericke znaky (a-z a 0-9).
Navic zadani primo vyziva k bruteforcu. Ze to nebude jen tak, zjistime zahy.

Metod na zjistovani (enumeraci) neznamich parametru v DNS je hodne, staci chvilka googleni: <https://pentester.land/cheatsheets/2018/11/14/subdomains-enumeration-cheatsheet.html>

Zkusime napred jednodusi (a najivnejsi) pristup. V Kali obsazeny dnsmap (<https://tools.kali.org/information-gathering/dnsmap>) by mohl neco ukazat:

```
root@kali:~# dnsmap try2hack.me
dnsmap 0.30 - DNS Network Mapper by pagvac (gnucitizen.org)

[+] warning: domain might use wildcards. 31.31.79.10 will be ignored from results
[+] searching (sub)domains for try2hack.me using built-in wordlist
[+] using maximum random delay of 10 millisecond(s) between requests

(zkraceno)...
a.try2hack.me
IPv6 address #1: 2a02:2b88:2:1::663d:1

aa.try2hack.me
IPv6 address #1: 2a02:2b88:2:1::663d:1

ab.try2hack.me
IPv6 address #1: 2a02:2b88:2:1::663d:1
....(zkraceno)

[+] 989 (sub)domains and 989 IP address(es) found
[+] completion time: 36 second(s)
```

At uz se zeptate na jakoukoli subdomenu, tak se vam vrati A/AAAA zaznam jak na IPv4, tak IPv6:

```
root@kali:~# dig naprostynesmysl.try2hack.me

; <<>> DiG 9.11.4-P2-3-Debian <<>> naprostynesmysl.try2hack.me
(zkraceno)...
;; ANSWER SECTION:
naprostynesmysl.try2hack.me. 1800 IN  CNAME  try2hack.me.
try2hack.me.    856  IN  A  31.31.79.10
...(zkraceno)
```

Hmmm, vypada to jako slepa cesta.. dnsmap vezme default wordlist a posled dotazy, server mu vrati cokoli o co si rekne.

Proc to tak je:

```
root@kali:~# dnsenum try2hack.me
dnsenum VERSION:1.2.4

-----   try2hack.me   -----


Host's addresses:
__________________

try2hack.me.                             608      IN    A        31.31.79.10


Wildcard detection using: amtdgczyehfn
_______________________________________

amtdgczyehfn.try2hack.me.                1800     IN    CNAME    try2hack.me.
try2hack.me.                             608      IN    A        31.31.79.10


!!!!!!!!!!!!!!!!!!!!!!!!!!!!

 Wildcards detected, all subdomains will point to the same IP address
 Omitting results containing 31.31.79.10.
 Maybe you are using OpenDNS servers.

!!!!!!!!!!!!!!!!!!!!!!!!!!!!
(zkraceno)
```

Na domene je nastaven wildcard zaznam, coz je ```*.try2hack.me```.
Nicmene, porad plati, ze jedna z 46656 domen je ta kterou hledame. Nevime, ale jak z ni dostat heslo.
Teoreticky ho lze napr. zapsat jako TXT zaznam, nebo bude subdomena pri slouzit jako web s heslem.

Po case jsem se k ukolu vratil. Je jasne ze primo v DNS datech tajne heslo nebude, tzn je potreba hledat na HTTP k subdomene.
Nejvice me potrapilo, najit vhodny zpusob na brute force utok spojeny s HTTP requestama.

Teoretiky to funguje tak, ze skript postupne vola tripismene subdomeny v URL (např. ```http://abc.try2hack.me```), pokud dostane odpoved
od serveru HTTP/1.1 301 Moved Permanently, tak je jasne, ze to neni hledana subdomena.
Ocekavana odpoved by byla HTTP/1.1 200 OK, tzn. na subdomene bude sidlit web a tam heslo.  

K vygenerevani vsech moznosti vede vice moznosti: skript v bashi, Pythonu...
pro neprogramatory (jako jsem já :) je v kali nastroj ```crunch``` ke generovani kombinaci pismen a cisel (<https://tools.kali.org/password-attacks/crunch>)

```
root@kali:~# crunch 3 3 abcdefghijklmnopqrstuvwxyz1234567890
Crunch will now generate the following amount of data: 186624 bytes
0 MB
0 GB
0 TB
0 PB
Crunch will now generate the following number of lines: 46656 
aaa
aab
...
```

Vysledek ulozime do souboru (celkem 46656 kombinaci)

Nasledne pouzijeme ```curl``` k postupnemu HTTP volani jednotlivych domen, stazena data ulozime do souboru

Asi by slo udelat nejakou elegantni smyckou, ale jak jsem psal nejsem programator.
Takze jsem stvoril neco takoveho: 
```
# sh task6.sh 2>/dev/null > task6.log
```
kde v ```task6.sh``` jsou ulozeny vsechny volane subdomeny doplnene o prikaz ```curl```

Priklad odpovedi:
```
root@kali:~# curl aaa.try2hack.me
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://try2hack.me/">here</a>.</p>
</body></html>
```
A nakonec najdeme heslo celkem jednoduse:

```
# cat task6.log | grep Password
```
```
Password: Bir63Fpw0d9MX
```

Ukol c. 10 [vyreseno]
---
Analyza sitoveho provozu ze zachyceneho PCAP dumpu, takova klasika vsech soutezi a CTF. Dalsi informace nejsou zadany, takze musime zacit naslepo.
Bezne projeti dumpu ve Wiresharku neodhaluje nic extra, vypada to na standardni provoz, spousta DNS, TCP a TLS spojeni. Pravdepodobne ale pujde jen o zamaskovani zajimavejsich paketu.

Wireshark umi vyhledavat v paketech zadany string, takze zkousim co me napadne jako prvni, tzn.: "password", "admin" atd... ale, jak jinak, nikam to nevede.

Zkousim se zamerit na "specialnejsi" protokoly a do oka mi padne VoIP komunikace SIPu:
```
10115  2019-04-14 15:09:01,089695  54.37.202.229  10.0.2.15  SIP  630  Request: MESSAGE sip:t2hme2@192.168.60.110:46195 |  (text/plain) 
```

Tohle uz je zajimaveji. Odchyceny frame vypada takto:

```
Frame 10115: 630 bytes on wire (5040 bits), 630 bytes captured (5040 bits)
Ethernet II, Src: RealtekU_12:35:02 (52:54:00:12:35:02), Dst: PcsCompu_87:28:75 (08:00:27:87:28:75)
Internet Protocol Version 4, Src: 54.37.202.229, Dst: 10.0.2.15
User Datagram Protocol, Src Port: 5060, Dst Port: 5060
Session Initiation Protocol (MESSAGE)
    Request-Line: MESSAGE sip:t2hme2@192.168.60.110:46195 SIP/2.0
        Method: MESSAGE
        Request-URI: sip:t2hme2@192.168.60.110:46195
        [Resent Packet: False]
    Message Header
        Via: SIP/2.0/UDP 54.37.202.229;rport;branch=z9hG4bK.eB2DKBj1UQ7579204Q9K2N0jme
        Via: SIP/2.0/UDP 10.0.2.15:5060;branch=z9hG4bK.D~bBnWA~j;rport=53341;received=84.16.132.176
        Max-Forwards: 69
        From: <sip:t2hme1@sip.linphone.org>;tag=HRzJx34Fe
        To: <sip:t2hme2@sip.linphone.org>
        Call-ID: 1FUgCSG-tI
        [Call-ID: 1FUgCSG-tI]
        CSeq: 20 MESSAGE
        Date: Sun, 14 Apr 2019 13:09:01 GMT
        User-Agent: Linphone Desktop/4.1.1 (belle-sip/1.6.3)
        Supported: replaces, outbound
        Content-Type: text/plain
        Content-Length: 53
    Message Body
        Line-based text data: text/plain (1 lines)
            S.e.c.r.e.t p.a.s.s.w.o.r.d: mNhr6sW9cs0sD4sVoVpwjf6C
```
A je to! Máme heslo pekne v plaintextu :)

```
Password: mNhr6sW9cs0sD4sVoVpwjf6C
```

Ukol c. 11 [vyreseno]
---
V zadani je primo napoveda, ze jde o slovnikovi utok na HTTP auth. formular.
Uzivatelske jmeno sice neni znamo, ale pokud vime, ze jde o webovou administraci, dost pravdepodobne bude login "admin". Je to sice strileni od boku, ale proc to nezkusit.

Na tuto praci se bude hodit nastroj ```hydra```, standardne obsazeny v Kali Linuxu a nejaky
pekny wordlisty. V Kali uz nejake sice jsou v ```/usr/share/wordlists```, ale lepsi bude zacit necim
lehcim a tedy i rychlejsim, tzn. seznamem 10000 nejcastejsich hesel.

Jdeme na to...url na ktere utocime je <https://try2hack.me/manage/>.
V hydre nezapomenout pouzit port 443 a prepinac -S pro SSL komunikaci.

```
root@kali:/usr/share/wordlists# hydra -l admin -P 10000_common_passwords -S -s 443 -f try2hack.me http-get /manage/
Hydra v8.8 (c) 2019 by van Hauser/THC - Please do not use in military or secret service organizations, or for illegal purposes.

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2019-04-16 13:02:46
```
...a po chvilce:

```
[DATA] max 16 tasks per 1 server, overall 16 tasks, 10000 login tries (l:1/p:10000), ~625 tries per task
[DATA] attacking http-gets://try2hack.me:443/manage/
[443][http-get] host: try2hack.me   login: admin   password: falcon
[STATUS] attack finished for try2hack.me (valid pair found)
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2019-04-16 13:03:15
```

Uspech. Login je opravdu ```admin``` a heslo je ```falcon```. Nyni uz staci otevrit URL v prohlizeci, zadat credentials a vyzvednout si kod...

```
Password: Veinsg5Vskg2Fpcb 
```


Ukol c. 14 [vyreseno]
---
Na serveru try2hack.me pry bezi nejake sitove uloziste, ktere mame objevit a tim se dostat i k heslu.
Jelikoz nevime vubec nic, tak zacneme nmap-em:

```
root@kali:~# nmap -sV try2hack.me
Starting Nmap 7.70 ( https://nmap.org ) at 2019-04-17 12:16 CEST
Nmap scan report for try2hack.me (31.31.79.10)
Host is up (0.019s latency).
Other addresses for try2hack.me (not scanned): 2a02:2b88:2:1::663d:1
Not shown: 984 closed ports
PORT      STATE    SERVICE          VERSION
22/tcp    open     ssh              OpenSSH 7.4p1 (protocol 2.0)
25/tcp    filtered smtp
80/tcp    open     http             Apache httpd (Nette Framework)
111/tcp   open     rpcbind          2-4 (RPC #100000)
443/tcp   open     ssl/ssl          Apache httpd (SSL-only mode)
513/tcp   filtered login
514/tcp   filtered shell
2049/tcp  open     nfs_acl          3 (RPC #100227)
2710/tcp  filtered sso-service
3000/tcp  filtered ppp
6901/tcp  filtered jetstream
7000/tcp  filtered afs3-fileserver
9000/tcp  filtered cslistener
9091/tcp  filtered xmltec-xmlmail
10000/tcp filtered snet-sensor-mgmt
49160/tcp filtered unknown

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.85 seconds
```

Hledame sluzbu sdileni dat, takze nas zaujmou porty ```111/tcp``` a ```2049/tcp```, coz napovida, ze na serveru bezi NFS (<https://en.wikipedia.org/wiki/Network_File_System>)

Zjistime vzdalene moutpointy:

```
root@kali:~# nmap -sV --script=nfs-showmount try2hack.me

(zkraceno)...
111/tcp   open     rpcbind          2-4 (RPC #100000)
| nfs-showmount: 
|_  /var/nfsroot *
| rpcinfo: 
|   program version   port/proto  service
|   100000  2,3,4        111/tcp  rpcbind
|   100000  2,3,4        111/udp  rpcbind
|   100003  3,4         2049/tcp  nfs
|   100003  3,4         2049/udp  nfs
|   100005  1,2,3      35645/udp  mountd
|   100005  1,2,3      40031/tcp  mountd
|   100021  1,3,4      34149/tcp  nlockmgr
|   100021  1,3,4      46179/udp  nlockmgr
|   100227  3           2049/tcp  nfs_acl
|_  100227  3           2049/udp  nfs_acl
(...zkraceno...)
2049/tcp  open     nfs_acl          3 (RPC #100227)
...(zkraceno)

```
Jak to vypada, tak na ```111/tcp``` opravdu neco je.
Vzdaleny "disk" mountneme u sebe. 

```
root@kali:~# mount -t nfs try2hack.me:/var/nfsroot /mnt/remote/
mount.nfs: Network is unreachable
```
Huh, coze? Zkousim hledat kde je problem. Zkousim jine zpusoby mountnuti,
ale vysledek je bud "Network is unreachable" nebo "access denied by server".
To mi prilis nedava smysl. Tusim problemy s Kali VM, takze pokus na Ubuntu 16.4,
bez vysledne. Nakonec zkousim server s public IP a tam se nakonec zadari


Do ```/etc/fstab``` pridame:
```
31.31.79.10:/var/nfsroot /mnt/remote nfs auto 0 0
```
a nasledne:
```
# mount /mnt/remote
```
```
# ls -la /mnt/remote/
total 12
drwxr-xr-x 2 nobody nogroup 4096 Apr 13 17:43 .
drwxr-xr-x 4 root   root    4096 Apr 30 06:20 ..
-rwxr-xr-x 1 nobody nogroup   17 Apr 13 17:43 Password.txt
```
```
# cat /mnt/remote/Password.txt
Ciw27xDowP20eXnv
```
Az po vyreseni koukam na forum: <https://try2hackme.activeboard.com/t65488843/public-ip-access/>,
takze jsem nebyl jediny, kdo na tomto ukolu zakufroval.

```
Password: Ciw27xDowP20eXnv
```