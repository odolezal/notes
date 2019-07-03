**ZSIS CTF** 
==========
Web hry: <https://ctf.zsis.hr>

Hráno pod nickem: ```Ondrashack``` od 16.5.2019. 

**Stav k 3.7.2019**: vyřešeno 7/53 úloh.

Kontakt: <https://www.odolezal.cz>

Poltergeist
---
* Challenge: Poltergeist
* Type: Trivia
* Points: 1

Něco na rozehřátí. Že by flag ukrytý ve zdrojovém kódu? Tentokrát ne. Popis zní
> You just missed your first flag :(

Takže je nutné se asi podívat trochu zpátky. Přiznávám, vůbec jsem nechápal kde hledat. Shodou okolností jsem došel na to, že když si otevřete homepage a nejste přihlášeni, zobrazí se krátké povídání o CTF:

> Information System Security Bureau hosts this Jeopardy-style CTF platform in an attempt to give information security enthusiasts a competitive environment to sharpen and prove their hacking (offensive) and incident handling (defensive) skills. Challenges are scored based on difficulty, where each player is proving that he solved it by submitting an answer in form of a flag ***(e.g. FLAG-{THEY_4R3_H3RE})***. There are no restrictions on who can join and there are no time limits on the whole competition. Each month there should be set of new challenges included, giving players more ways to climb up the scoreboard. For more questions and support contact us on ctf@zsis.hr. Also, you can join other players at the official Slack channel ZSIS CTF.

a součástí toho textu je i příklad FLAGu. A tento FLAG je validní pro první challenge. Někdy opravdu není nutné hledat složitosti.

Password: ```FLAG-{THEY_4R3_H3RE}```

Inglorious
---
* Challenge: Inglorious
* Type: Steganography
* Points: 2 

Odkaz challenge vede na JPG obrázek (<https://ctf.zsis.hr/challenges/2_stego_inglorious.jpg>). Na první pohled samozřejmě není vidět nic zajímavého. FLAG bude ukryt někde "uvnitř". Průzkum EXIF dat nepřinesl nic užitečného. 

Následně se nabízí udělat jednoduše Hex dump z obrazového souboru. Na to použijeme nástroj ```xxd``` (viz 
<https://www.poftut.com/use-linux-xxd-command-tutorial-hex-binary-operations-example/>):

```root@kali:~# xxd 2_stego_inglorious.jpg```

A na konci dump-u najdeme FLAG:

```
...
00025730: 0ba2 08ff d900 4600 4c00 4100 4700 2d00  ......F.L.A.G.-.
00025740: 7b00 4200 5500 5300 4900 4e00 4500 5300  {.B.U.S.I.N.E.S.
00025750: 5300 5f00 3100 5300 5f00 4100 5f00 4200  S._.1.S._.A._.B.
00025760: 3000 3000 4d00 4900 4e00 7d00            0.0.M.I.N.}.
```

Password: ```FLAG-{BUSINESS_1S_A_B00MIN}```

Player One
---
* Challenge: Player One
* Type: Trivia 
* Points: 2 

V zadaní je PNG file QR kódu který je ale zjevně upravený, tzn. nelze jej přímo naskenovat/načíst QR čtečkou. Při porovnání s jakýmkoli jiným QR kódem je vidět, že v levém horním rohu je něco, co tam být nemá. Struktura QR kódu je popsána např. zde <https://en.wikipedia.org/wiki/QR_code#/media/File:QR_Code_Structure_Example_3.svg>.

Obrázek stačí stáhnout a v obrázkovém editoru upravit QR kód: tzn. odstranit obrázek z levého horního rohu a nahradit ho černým čtvercem z rohu pravého nebo dolního levého. Upravený korektní QR kód lze dekódovat online např. zde <https://zxing.org/w/decode.jspx> a tak dostaneme FLAG.


Password: ```FLAG-{F1RST_T0_THE_3GG}```

Vatreni
---

* Challenge: Vatreni
* Type: Networking
* Points: 3

Máme zachycenou šifrovanou komunikaci protokolu HTTPS. K zadání je přiložen privátní klíč serveru, takže dešifrování v celku jednoduchou záležitostí.

Ve Wiresharku otevřeme přiložený ```capture.pcapng```. Načteme privátní klíč serveru v ```Edit > Preference > Protocols > TLS```.

Následně u položky ```RSA keys list``` klikneme ```Edit...```. Otevře se nové okno ```TLS Decprypt```. Přidáme novou položku přes ```+``` v dolní části a vyplníme:

```
IP address: 172.16.112.134
Port: 443
Protocol: http
Key File: (cesta ke staženému site.pem souboru)
```

Wireshark následně dešifruje HTTPS komunikaci zadaným klíčem. Ve výpisu se zobrazí dešifrovaná komunikace. Můžeme aplikovat filtr na ```http```.

Zajímavá je tato odpověď serveru:

```
HTTP/1.1 200 OK
Date: Wed, 18 Jul 2018 11:06:38 GMT
Server: Apache/2.4.10 (Debian)
Vary: Accept-Encoding
Content-Encoding: gzip
Content-Length: 412
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/html;charset=UTF-8
```

A stránka HTML:

```
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
<html>
 <head>
  <title>Index of /</title>
 </head>
 <body>
<h1>Index of /</h1>
  <table>
   <tr><th valign="top"><img src="/icons/blank.gif" alt="[ICO]"></th><th><a href="?C=N;O=D">Name</a></th><th><a href="?C=M;O=A">Last modified</a></th><th><a href="?C=S;O=A">Size</a></th><th><a href="?C=D;O=A">Description</a></th></tr>
   <tr><th colspan="5"><hr></th></tr>
<tr><td valign="top"><img src="/icons/image2.gif" alt="[IMG]"></td><td><a href="flag.jpg">flag.jpg</a></td><td align="right">2018-07-18 13:06  </td><td align="right">265K</td><td>&nbsp;</td></tr>
   <tr><th colspan="5"><hr></th></tr>
</table>
<address>Apache/2.4.10 (Debian) Server at 172.16.112.135 Port 443</address>
</body></html>
```

Jak je vidět, v přenesené HTML stránce je odkaz na ```flag.jpg```. Vyextrahujeme JPG file pomocí ```File > Exports Object > HTTP...```

Zobrazí se seznam objektů k exportu: ```HTTP object list```, vybereme ```flag.jpg``` a uložíme. V JPG souboru je obsažen hledaný flag.

Password: ```FLAG-{CROAT14N_H3ROE5}```

_Poznámka pod čarou: V tomto případě byla challenge zjednodušena tím, že byl privátní klíč přiložen k souboru se zachycenou komunikací. Lze se i setkat s challengemi (např. zde: <https://hatsoffsecurity.com/2018/10/30/decrypting-traffic-in-wireshark/>) kde privátní klíč nemáte a musíte podle různých vodítek správný klíč najít různě po internetu. Pro účely CTF se používají testovací, dočasné nebo mimoprodukční privátní klíče. Za běžných okolností nikdy nesmí privátní klíč uniknout ze serveru, jelikož by v tom případě veškeré zabezpečení bylo naprosto neúčinné._

Clones
---

* Challenge: Clones 
* Type: Web
* Points: 4 

Popis challenge ```Black-box testing``` a kategorie ```Web``` dává tušit, že budeme hledat něco skrytého na webové stránce.

Web <https://ctf.zsis.hr/challenges/4_web_clones.php> v prohlížeči zobrazí všeříkající 
>Hello, World!

Spustíme webový skener ```nikto```, který se na "black box" testování hodí skvěle:
```
root@kali:~# nikto -h https://ctf.zsis.hr/challenges/4_web_clones.php
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          31.45.240.57
+ Target Hostname:    ctf.zsis.hr
+ Target Port:        443
---------------------------------------------------------------------------
+ SSL Info:        Subject:  /CN=ctf.zsis.hr
                   Ciphers:  ECDHE-RSA-AES128-GCM-SHA256
                   Issuer:   /C=US/O=Let's Encrypt/CN=Let's Encrypt Authority X3
+ Start Time:         2019-06-25 12:13:29 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache
+ The site uses SSL and Expect-CT header is not present.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Uncommon header 'x-firewall-blocked' found, with contents: FLAG-{M4Y_THE_F0RC3_BE_W1TH_YOU}
...
```
Na 4. řádku skenu je detekována neobvyklá HTTP hlavička ```x-firewall-blocked``` která nepřekvapivě obsahuje hodnotu s FLAGem.

Password: ```FLAG-{M4Y_THE_F0RC3_BE_W1TH_YOU}```

White Noise
---

* Challenge: White Noise 
* Type: Web
* Points: 3 

Stránka <https://ctf.zsis.hr/challenges/3_web_noise.php/> obsahuje už na první pohled nesmyslnou změť znaků: 
```
bv8KGsaI7BXS6QyCPBTfN8vFSfJOW6q8CzSi1309FY2MPBoEciUZrpFjFp8CvyK7
7Dq9Hrjmpm8fXxT9QO9hdPATeIvKhfSpTiyAJRW9e5obDikt6uLkjmdy4IjmYbLR
tksdcpnquMB84WCbqnwKJJjOsCarNWjhgLvsaSSFFuNKqpVRNsBwbVlExw5ksoCJ
97bk04ZFyNqZdlQ0Osx0nTEVpJgS8SBi0ND0RCGqq6pEsgEgJcg75V2vFinNaY6a
LJbDmR4MYuqqK5HuiXBoTDTyVgl6fsh0bsEykIlidMJYRqsao3yiHrQDIcKXE1YQ
uCoOlJ7yvRwniZxH36ZKyPng17eG9cwDOUsaEAJ9rgwJf4riaq3Jfq0hyeXIqtlf
oOp2p9bQpIAFN1XYs1HHsHY0VWJlp5BOT1Qia19AKKfxMdveedWGUUHQRqchvN5p
OVHZXRzHBOfn1LCfYyWTtDJk3VBzJGYxBGxzx6g9UwxWh9bgH8aaLTuOP5ozMm7n
2EXAKdKEKhB2qNj7VtiGmMvcSTLEgS2jwZThdEVYVw0mkjufMMV9zqmrk76A09Tx
8NOmrKlmhlIBFcQsZLBycYZw5666f0DoNsLec6Atsj47wVzvGb4T93qfaxmpx0Ok
szyEF987tdf08PvP0zJaD9qOGMddM2xfB6Thg2oKfEKntgduPWEt54hLRvZExwT9
DNqUPPE4tosWEFquC5YHagt1LsGjZzsDmTxbIcgcAI8fozJ0FIIPYbRKEx3E7wht
pOF80VkBEtQ22A2HjKxhWo1BW5f3Cwx2ldam9uYNYPP0pRIJCg0zF2aB7pFKWdMh
qWEArCnprcqR39AGpAf5DqHLPmvMAh31eIBGkY6LawCeGdV6NabrBScqfHdPZhRd
ZsUjq05BxIPdVKkJVvawonXD4at4skhrMcLdcQOKzEXvphekNpRbMPPR0iVsCdUp
pFCCwrm65kBuCQPpfHA2xpTxIO0k2UKrAm46NqcTLNonDdMTVnVsNP0vE0QGUB8u
                 NOISE IS JUST THE DISTRACTION!
XcBLDOFoC3Lghy9cW5FJVFfzF6gAHo5FBGrfu6E7aqnrYwEUCjExYU7D0neHMjno
ZOEuViB5IYxHvbC8uhGtbN7bblTYEgmE5090jK61JDJfPlnjC3NNRUZ2gS0U9nzf
oIgHtmId0rsPNQ9qTWdLRdN76O2gcBvAkLhN8009stYgj7Gc4UYV7M3eB5uNHZo1
LGPUHP39j2qCa6Pe0Na8zdmajRY0Qm1C2RxJHAT0CkCMqr1rebAOpXZIOXJFjLhm
CP6jp0j2kWPLnQdC2NrrKqaznTeHFw4hlaBLbUNwRChftuSvijX3K7C71RPGnTYI
4ztguhMlT4AmzsSRMPVwXxEZotGMnEuseYIIgv49zEw87oZTeVpbs4aRxREVv9oJ
76snBwxba4jitjbHeBTHG3ydUc9qlxatECRg8pritKBW3NDipwZ5AxjvKsV606AE
IrUQQm9j6KgaxTsWqs11ZkwKNsRNyrsgSm7JIg3P1j0ydtvDVxEVSbGGDxtcZVtR
iAB1QERSYSrblWPhuudnFT3jrxvqtZiLzTNqyEiwwJISGxab1nyHhC0J9w9Cvso5
mbvUQNrnxafdHqoJNXr5zsOJYYluqJzMV5HLT88qioE0O3KC0cHAEwjDuF8UpIHk
No6Gwe7PDLQrPB4POLqsiJ6MpeHOWo8KMerjty96k0y9CCZqopTG9ZtydanazvUm
KlGdUPkeQSotvnTUNMAXL4wZeTaOp4a9qQnlGHAxAY06mU0aGB8sFEsUyCIXHT78
Jutqc3YM2YToSTyzuH29lu3U7MROFYWptqPFtNswMlUFfteJagTwLWqSIiGnhDNK
3CqxqT3deYSus7dDo6993A2MSIaamXVqAlY1e2ft07Xtfb6DigNlQP8JyjTUgOkR
aiTol8SlgPPv0V9icWE2MNMk6GfnuAeFT74efWAvMp1Mla5x7KATxmeD2t0x4fcX
ngbCdL8Zb9LwkR4rBEl91zM33NB73O4q5g3i2chdm3KGUO8wstFu3sx6g9dkXiK2
```

ty se navíc při každým načtení změní, takže není nic překvapivého, že obsah webu je úplně k ničemu. Název a popis challenge (```Ignore the noise```) to přímo říká.

Skener ```nikto``` běží celkem dlouho, ale stránka vrací samé false positive výsledky (záměr autorů?), takže tudy cesta nevede. To nejzákladnější (navíc, s ohledem na tento konkrétní typ challenge) je podívat se na HTTP hlavičky v odpověď na prostý dotaz pomocí ```curl```:

```
root@kali:~# curl -I https://ctf.zsis.hr/challenges/3_web_noise.php

HTTP/1.1 200 OK
Date: Tue, 25 Jun 2019 11:59:00 GMT
Server: Apache
X-Frame-Options: SAMEORIGIN
X-Content-Type-Options: nosniff
X-XSS-Protection: 1; mode=block
Referrer-Policy: same-origin
Set-Cookie: PHPSESSID=cn4mqg400h007i5ps0gg38c356; path=/; HttpOnly
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Warning: FLAG-{NO_ON3_SEES_THE_B4RN}
Strict-Transport-Security: max-age=31536000
Content-Type: text/html; charset=UTF-8
```

Server posílá hlavičku ```Warning``` (což je standardní, zdokumentovaná hlavička, viz <https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Warning>) s hodnotou FLAGu.

Password: ```FLAG-{NO_ON3_SEES_THE_B4RN}```

Zorba
---

* Challenge: Zorba 
* Type: Web
* Points: 4

Po otevření odkazu <https://ctf.zsis.hr/challenges/4_web_zorba.php> se zobrazí text:
> Setup your dial-up modem and come back here from the IP address located in 'Rwanda' 
  p.s. Rules: you have 30 sec to solve the challenge
 
Takže musíme stránku otevřít z IP adresy geolokačně patřící do Rwandy a máme na to jen 30 sekund. Tato challenge pro každý pokus generuje jinou zemi. Řešením je použít web proxy umístěnou v dané zemi. Web ctf.zsis.hr používá HTTPS takže potřebujeme server umožnující transparentně přeposílat HTTPS komunikaci. Navíc, je nutné být přihlášen na webu ctf.zsis.hr takže nelze použít ```curl``` ale plnohodnotný webový prohlížeč. Nastavení např. ve Firefoxu: <https://support.mozilla.org/cs/kb/nastaveni-pripojeni-ve-firefoxu>

Seznam proxy serverů např. zde: <http://free-proxy.cz/en/proxylist/country/all/https/ping/all>.
Úspěch záleží hlavně na tom najít rychle funkční proxy, nastavit připojení a stihnout to do 30 sekund. Generované země jsou někdy více někdy méně exotické, takže záleží na štěstí.

Password: ```FLAG-{TO_B3_ALIVE_IS_T0_UNDO_YOUR_BELT_AND_L0OK_FOR_TROUBLE}```