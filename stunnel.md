# stunnel

## Úvod

V tomto článku je představena ukázková implementace programu [**stunnel**](https://www.stunnel.org/), který zprostředkovává zabezpečené spojení, resp. šifrovaný tunel (vystupuje v roli TLS Proxy serveru) pro streamovací program **motion** s důvěryhodnými TLS certifikáty od **Let's Encrypt**. Použitý systém je **Debian 9 (Stretch)**

## Motivace

Program [motion](https://motion-project.github.io/) umožňuje streamovat obraz z video zařízení (webkamera, vstupní video karta, DVB-T tunner,...) po síti. Nejčastěji se využívá protokol HTTP, který je ale nezabezpečený. Program sice umožňuje zapnout ověřování pomocí uživatelského jména a hesla, ale nemá v sobě přímo implementovaný HTTPS server pro bezpečné spojení.

Praktické řešení tohoto problému je, že se klient nejprve bezpečně připojí pomocí HTTPS na jíný, "předřazený" TCP port. To obstará `stunnel`. Následeně se data rozšifrují a předají se koncovému programu, zde tedy démonu `motion`. Předání probíhá pouze v rámci serveru, takže nelze data například odposlechnout, nebo podvrhnout. 

Jako bonus budeme používat bezplatné, důvěryhodné certifikáty od [Let's Encrypt](https://letsencrypt.org/). Postupů jak certifikáty získat, je více. Já jsem postupoval podle tohoto: [How To Use Certbot Standalone Mode to Retrieve Let's Encrypt SSL Certificates on Debian 9](https://www.digitalocean.com/community/tutorials/how-to-use-certbot-standalone-mode-to-retrieve-let-s-encrypt-ssl-certificates-on-debian-9). 


`stunnel` sice umožňuje pracovat i se self-signed certifikáty, tím ale vystavujeme klienta riziku podvrhnutí identitty serveru, ke kterému se připojuje.

`stunnel` využívá knihovnu `OpenSSL`, kterou je samozřejmě nutné mít nainstalovanou.

## Implementace

Instalace se provede standardně:
```
sudo apt install stunnel4
```

Ve výchozím nastavené neběží `stunnel` jako démon (služba). Pokud chceme mít `stunnel` jako službu, je nutné v souboru `/etc/default/stunnel4` přepnout parametr `ENABLED=0` na `ENABLED=1`:

```
# Change to one to enable stunnel automatic startup
ENABLED=1
FILES="/etc/stunnel/*.conf"
OPTIONS=""
```

Díky tomu bude `stunnel` automaticky spuštěn po startu systému a bude hledat konfiguraci (jakýkoli soubor s příponou `*.conf`) v umístění `/etc/stunnel/`. 

Vytvoříme soubor `/etc/stunnel/stunnel.conf`:

```
# stunnel config file
```
Soubor bude obsahovat následující řádky konfigurace:

Definujeme PID file pro naši instanci:
```
pid = /var/run/stunnel4/stunnel.pid
```

Definujeme soubor pro ukládání logu:
```
output = /var/log/stunnel4/stunnel.log
```

Po spuštění předáme práva uživateli `stunnel4`, který je již vytvořen po instalaci balíčku:
```
setuid = stunnel4
setgid = stunnel4
```

Doplňková konfigurace, která by měla "podržet" spojení na pomalých linkách:
```
socket = r:TCP_NODELAY=1
socket = l:TCP_NODELAY=1
```

Úroveň debug výstupu do logu. Pro prvotní ladění se doporučuje `debug = 4` nebo `debug = 5`.
```
debug = 3
```

Založíme novou instanci šifrovaného tunelu pro démona `motion`: 
```
# Tunneling "motion" deamon HTTP stream
[motion]
```

Vytvoříme síťový socket na portu `tcp/8082`. Naslouchá na libovolné IP adrese pro příchozí spojení:
```
accept = 0.0.0.0:8082
```

`stunnel` připojíme lokálně k portu `tcp/8081` na kterém standardně naslouchá stream démonu `motion`:
```
connect = 127.0.0.1:8081
```

Cesta k úplnému řetězci certifikátů Let's Encrypt, které jsme předtím získali. Je nutné použít opravdu celý řetězec a ne jen koncový certifikát `cert.pem`:
```
cert = /etc/letsencrypt/live/example.com/fullchain.pem
```

Cesta k vygenerovanému privátnímu klíči:
```
key = /etc/letsencrypt/live/example.com/privkey.pem
```

Nastavíme množinu chtěných šifer. Toto nastavení odpovídá úrovni _Intermediate_  pro verzi `OpenSSL 1.1.1d` z [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/#server=apache&version=2.4.41&config=intermediate&openssl=1.1.1d&hsts=false&ocsp=false&guideline=5.4). Syntaxe je kompatibilní s konfigurací `apache 2.4.41`:
```
ciphers = ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384
```

Vypneme nechtěné SSL/TLS verze protokolu a nastavíme preferenci šifer na straně serveru. `stunnel` ve výchozím stavu preferuje `TLSv1.2`:
```
options = NO_TLSv1.1
options = NO_TLSv1
options = NO_SSLv3
options = CIPHER_SERVER_PREFERENCE
```

Vypne možnost znovu vyjednání parametru TLS spojení:
```
renegotiation = no
```

## Testování

Kontrola, zda deamon běží a načetl platnou konfiguraci:
```
user@server:~ $ sudo service stunnel4 status
● stunnel4.service - LSB: Start or stop stunnel 4.x (TLS tunnel for network daemons)
   Loaded: loaded (/etc/init.d/stunnel4; generated; vendor preset: enabled)
   Active: active (running) since Wed 2020-03-11 17:54:35 CET; 1s ago
     Docs: man:systemd-sysv-generator(8)
  Process: 1209 ExecStop=/etc/init.d/stunnel4 stop (code=exited, status=0/SUCCESS)
  Process: 13336 ExecReload=/etc/init.d/stunnel4 reload (code=exited, status=0/SUCCESS)
  Process: 1232 ExecStart=/etc/init.d/stunnel4 start (code=exited, status=0/SUCCESS)
    Tasks: 1 (limit: 4915)
   CGroup: /system.slice/stunnel4.service
           └─1248 /usr/bin/stunnel4 /etc/stunnel/stunnel.conf

Mar 11 17:54:34 server systemd[1]: Starting LSB: Start or stop stunnel 4.x (TLS tunnel for network daemons)...
Mar 11 17:54:35 server stunnel4[1232]: Starting TLS tunnels: /etc/stunnel/stunnel.conf: started
Mar 11 17:54:35 server systemd[1]: Started LSB: Start or stop stunnel 4.x (TLS tunnel for network daemons).
```

V tuto chvíli by měla mít instance TLS tunelu vytvořený síťový port:
```
user@server:~ $ netstat -anut | grep :8082
tcp        0      0 0.0.0.0:8082            0.0.0.0:*               LISTEN
```

Test TCP spojení klient-server pomocí utility `netcat`:
```
user@client:~ $ nc -v 10.0.0.100 8082
Connection to 10.0.0.100 8082 port [tcp/*] succeeded!
```

Pomocí `curl` lze ověřit HTTPS spojení a to, jestli máme správně nastavené certifikáty. Kvůli ověření certifikátu serveru je nutné zadávat celé doménové jméno (fully qualified domain name (FQDN)):
```
user@client:~$ curl -v -I https://example.com:8082
...
-redigováno-
* TLSv1.3 (OUT), TLS handshake, Client hello (1):
* TLSv1.3 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Client hello (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES128-GCM-SHA256
-redigováno-
* Server certificate:
-redigováno-
*  issuer: C=US; O=Let's Encrypt; CN=Let's Encrypt Authority X3
*  SSL certificate verify ok.
> HEAD / HTTP/1.1
-redigováno-
> User-Agent: curl/7.58.0
> Accept: */*
>
* HTTP 1.0, assume close after body
< HTTP/1.0 501 Method Not Implemented
HTTP/1.0 501 Method Not Implemented
< Content-type: text/plain
Content-type: text/plain
...
```

Serverem nabízené šifry lze otestovat skenem v rámci `nmap`u:
```
user@server:~ $ nmap --script ssl-enum-ciphers localhost -p 8082

Starting Nmap 7.40 ( https://nmap.org ) at 2020-03-11 18:20 CET
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00035s latency).
Other addresses for localhost (not scanned): ::1
PORT     STATE SERVICE
8082/tcp open  blackice-alerts
| ssl-enum-ciphers:
|   TLSv1.2:
|     ciphers:
|       TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (secp256r1) - A
|       TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (secp256r1) - A
|       TLS_DHE_RSA_WITH_AES_128_GCM_SHA256 (dh 2048) - A
|       TLS_DHE_RSA_WITH_AES_256_GCM_SHA384 (dh 2048) - A
|     compressors:
|       NULL
|     cipher preference: server
|_  least strength: A

Nmap done: 1 IP address (1 host up) scanned in 2.43 seconds
```

Podobně lze využít přímo `OpenSSL` v režimu klienta:
```
user@client:~$ openssl s_client -showcerts -connect example.com:8082 </dev/null
CONNECTED(00000005)
depth=2 O = Digital Signature Trust Co., CN = DST Root CA X3
verify return:1
depth=1 C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
verify return:1
depth=0 CN = -redigováno-
verify return:1
---
Certificate chain
 0 s:CN = -redigováno-
   i:C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
-----BEGIN CERTIFICATE-----
...
-redigováno-
...
-----END CERTIFICATE-----
 1 s:C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3
   i:O = Digital Signature Trust Co., CN = DST Root CA X3
-----BEGIN CERTIFICATE-----
...
-redigováno-
...
-----END CERTIFICATE-----
---
Server certificate
-redigováno-

issuer=C = US, O = Let's Encrypt, CN = Let's Encrypt Authority X3

---
No client certificate CA names sent
Peer signing digest: SHA512
Peer signature type: RSA
Server Temp Key: ECDH, P-256, 256 bits
---
SSL handshake has read 3071 bytes and written 447 bytes
Verification: OK
---
New, TLSv1.2, Cipher is ECDHE-RSA-AES128-GCM-SHA256
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
SSL-Session:
    Protocol  : TLSv1.2
    Cipher    : ECDHE-RSA-AES128-GCM-SHA256
    -redigováno-
    Session-ID-ctx:
    -redigováno-
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1583948204
    Timeout   : 7200 (sec)
    Verify return code: 0 (ok)
    Extended master secret: yes
---
DONE
```

## Zdroje
* <https://www.stunnel.org/faq.html>
* <https://www.stunnel.org/docs.html>
* <https://www.stunnel.org/static/stunnel.html>
* <https://fedoramagazine.org/securing-telnet-connections-with-stunnel/>
* <http://docs.eggplantsoftware.com/ePF/using/epf-vnc-with-stunnel.htm>
* <https://wiki.archlinux.org/index.php/Stunnel>
* <https://hamy.io/post/0012/how-to-install-and-configure-stunnel-on-ubuntu/>
* <https://www.loadbalancer.org/blog/stunnel-cipher-list-and-qualys-ssl-labs-testing/>
