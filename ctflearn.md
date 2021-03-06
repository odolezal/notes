**CTFlearn.com** 
==========
Web hry: <https://ctflearn.com>

Hráno pod nickem: ```Ondrashack``` od 30.5.2019. 

**Stav k 4.7.2019**: vyřešeno 9/X úloh.

Kontakt: <https://www.odolezal.cz>


Character Encoding
---
Velmi jednoduchá challenge. Je zadán řetězec znaků v hexadecimálním formátu ```41 42 43 54 46 7B 34 35 43 31 31 5F 31 35 5F 55 35 33 46 55 4C 7D```. Stačí tedy převést na ASCII string (např. <https://www.rapidtables.com/convert/number/hex-to-ascii.html>), dostaneme ```ABCTF{45C11_15_U53FUL}```

FLAG: ```45C11_15_U53FUL```


POST Practice
---
Název naznačuje, že se zaměříme na požadavek typu POST (<https://cs.wikipedia.org/wiki/POST>) v protokolu HTTP. Zadání říká, že na stránce <http://165.227.106.113/post.php> je vyžadována autentizace právě metody POST.

Na stránce jako takové nic zajímavého není
> This site takes POST data that you have not submitted!

Podíváme se do zdrojového kódu:

```<!-- username: admin | password: 71urlkufpsdnlkadsf -->```

Komentář v kódu nám zcela jasně odhaluje přihlašovací údaje. Nyní je stačí poslat pomocí ```cURL``` (<https://ec.haxx.se/http-post.html>) na server:

```curl -d "username=admin&password=71urlkufpsdnlkadsf" -X POST http://165.227.106.113/post.php```

A zpět dostaneme odpověď s flagem: ```flag{p0st_d4t4_4ll_d4y}```

FLAG: ```p0st_d4t4_4ll_d4y```


Don't Bump Your Head(er)
---
Podle zadání máme na stránce <http://165.227.106.113/header.php> obejít zabezpečení. Zadaní odkazuje na hlavičky HTTP protokolu (<https://en.wikipedia.org/wiki/List_of_HTTP_header_fields>).

Stránka po otevření zobrazí:
> Sorry, it seems as if your user agent is not correct, in order to access this website. The one you supplied is: Mozilla/5.0...

Tzn. vidíme, že musíme změnit hlavičku "User-Agent" na jinou, než standardně posílá náš webový prohlížeč. Ve zdrojovém kódu stránky je poznámka ```<!-- Sup3rS3cr3tAg3nt  -->```, takže víme co zadat jako hodnotu parametru "User-Agent". Pomocí ```cURL``` změníme hlavičku:

```curl -A "Sup3rS3cr3tAg3nt" http://165.227.106.113/header.php```

A server vrátí:

```Sorry, it seems as if you did not just come from the site, "awesomesauce.com".```

Ještě nejsme u cíle. Odpověď naznačuje, že je potřeba změnit ještě jednu hlavičku. Nápověda v odpovědi odkazuje na možnost, dát serveru vědět, odkud klient přichází. Podle dostupných HTTP hlaviček (viz link výše) najdeme hlavičku ```Referer```, její popis:
> This is the address of the previous web page from which a link to the currently requested page was followed.

Do ```cURL``` příkazu přidáme patřičný parametr ```-H``` a navíc ```-v``` pro podrobnější odpověď (není nutné) a odešleme:

```curl -v -H 'Referer: awesomesauce.com' -A "Sup3rS3cr3tAg3nt" http://165.227.106.113/header.php```

Podrobná odpověď:
```
* Expire in 0 ms for 6 (transfer 0x564de5317dd0)
*   Trying 165.227.106.113...
* TCP_NODELAY set
* Expire in 200 ms for 4 (transfer 0x564de5317dd0)
* Connected to 165.227.106.113 (165.227.106.113) port 80 (#0)
> GET /header.php HTTP/1.1
> Host: 165.227.106.113
> User-Agent: Sup3rS3cr3tAg3nt
> Accept: */*
> Referer: awesomesauce.com
> 
< HTTP/1.1 200 OK
< Server: nginx/1.4.6 (Ubuntu)
< Date: Fri, 07 Jun 2019 06:30:24 GMT
< Content-Type: text/html
< Transfer-Encoding: chunked
< Connection: keep-alive
< X-Powered-By: PHP/5.5.9-1ubuntu4.22
< 
Here is your flag: flag{did_this_m3ss_with_y0ur_h34d}
<!-- Sup3rS3cr3tAg3nt  -->
* Connection #0 to host 165.227.106.113 left intact
* 
```
FLAG: ```did_this_m3ss_with_y0ur_h34d```

IP Tracer
---
Hledáme město do kterého geograficky patří IP adresa ```159.167.16.5```. Použijeme jakoukoli "Geo IP" službu, např.: <https://www.geoiptool.com/en/?ip=159.167.16.5>.

FLAG: ``` London```

Forensics 101
---

Stáhneme přiložený obrázek a uděláme Hex dump:


```
root@kali:~# xxd /root/Downloads/95f6edfb66ef42d774a5a34581f19052.jpg 

....
000023a0: 9125 1df9 c23c 1a85 7488 ed0d 9b2f 19b1  .%...<..t..../..
000023b0: d7c6 28c6 0e66 6c61 677b 776f 7721 5f64  ..(..flag{wow!_d
000023c0: 6174 615f 6973 5f63 6f6f 6c7d 8512 53be  ata_is_cool}..S.
000023d0: 6759 470a d06c 1b07 7eb3 715b dc61 3554  gYG..l..~.q[.a5T
....
```

FLAG: ```wow!_data_is_cool```

QR Code
---

Zadaný QR kód není nijak upraven, takže jej lze bez problémů protáhnout jakýmkoli dekodérem. Dostaneme:  

```c3ludCB2ZiA6IGEwX29icWxfczBldHJnX2RlX3BicXI=```

Což je řetězec zakódovaný v Base64 (<https://cs.wikipedia.org/wiki/Base64>), ten lze opět dekódovat:

```synt vf : a0_obql_s0etrg_de_pbqr```

Dostáváme text který se již podobá formátu FLAGu, ale jako by byly znaky vyměněny či "přetočeny". Takže to vypadá na nějaký typ substituční šifry, zde konkrétně to je ROT13 (<https://cs.wikipedia.org/wiki/ROT13>). Dešifrováním získáme FLAG.

FLAG: ```n0_body_f0rget_qr_code```

QR Code v2
---

QR kód ukrývá odkaz na soubor ```Flag.txt``` ve kterém je "nečekaně" FLAG.

FLAG: ```2_QR_4_U```

Where Can My Robot Go?
---

V zadání je přímo několik nápověd, že už to snad ani lehčí být nemůže... Hned první:
> Hint: Where do robots find what pages are on a website? 

odkazuje na soubor ```robots.txt```. A ten skutečně existuje: <https://ctflearn.com/robots.txt> a obsahuje:

```
User-agent: *
Disallow: /70r3hnanldfspufdsoifnlds.html
```

Na URL <https://ctflearn.com/70r3hnanldfspufdsoifnlds.html> se nachází FLAG.

FLAG: ```r0b0ts_4r3_th3_futur3```

Paste Those Binaries
---

Název a zadání challenge odkazuje na "paste" a "bin". Takže domyslet si, že zadaný ID ```r44LxiXq``` patří ke službě Pastebin.com. FLAG je na URL <https://pastebin.com/r44LxiXq>.

FLAG: ```past3_that_b1n```
