**CTFlearn.com** 
==========
Web hry: <https://ctflearn.com>

Hráno pod nickem: ```Ondrashack``` od 30.5.2019. 

**Stav k 7.6.2019**: vyřešeno 4/X úloh.

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