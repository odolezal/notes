**ZSIS CTF** 
==========
Web hry: <https://ctf.zsis.hr>

Hráno pod nickem: ```Ondrashack``` od 16.5.2019. 

**Stav k 24.6.2019**: vyřešeno 1/X úloh.

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

a součástí toho textu je i příklad FLAGu. A tento FLAG je validní pro první challenge. Někdy opravdu není nutné hledat složitosti..

Password: ```FLAG-{THEY_4R3_H3RE}```

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