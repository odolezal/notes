**Hackable.ca** 
==========
Web hry: <https://hackable.ca>
> This website is targeted for beginner computer enthusiasts who want to expand their computing knowledge. By doing these challenges, you will be exposed to a wide range of major vulnerabilities to programming languages and learn how to prevent vulnerable applications in the real world. This CTF works on a point system, try get on the scoreboards! Good Luck! ;)

Hrano pod nickem: ```Ondrashack``` od 4.6.2019. 

**Stav k 5.6.2019**: vyreseno 3/10 uloh.

Kontakt: <https://www.odolezal.cz>

Web - KISS
---
Úplně první challenge za 1 bod? Co to tak asi bude? Ano, většina obecných CTF začíná tím, že hledáte vlajku ve zdrojovém kódu stránky a zde to nebyla výjimka.

FLAG: ```flag{k33p_1t_s1mp13_8tup1d}```

Web - User Agent
---
Z názvu challenge a popisu je jasné o co půjde. Stránka <http://hackable.ca/useragent.php> vyhodnocuje příznak "User Agent" u klienta. Pro získání flagu stačí změnit hodnotu User Agent-a na ```hackableisawesome```. 

Jak na to, např. zde: <http://osxdaily.com/2011/07/16/change-user-agent-with-curl/> 

Na příkaz:

```curl -A "hackableisawesome" http://hackable.ca/useragent.php```

nicméně server odpoví ```301 Moved Permanently```, takže je ještě potřeba říci ```curl```, ať následuje přesměrování:

```curl -L -A "hackableisawesome" http://hackable.ca/useragent.php```

a poté již sever vrátí flag.

FLAG: ```flag{pretty_3asy_r1ght}```

Web - Server Status
---
Máme stránku <http://web.hackable.ca:8082/> s přihlášením v PHP, ve zdrojáku nic zajímavého, podezřelé cookies taky nic. Nikde není přímo vidět jiné vodítko, takže bude potřeba web obšírněji proskenovat, na to se hodí nástroj ```nikto``` (<https://tools.kali.org/information-gathering/nikto>) obsažený v Kali Linuxu:

```
root@kali:~# nikto -h web.hackable.ca:8082
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          50.3.65.14
+ Target Hostname:    web.hackable.ca
+ Target Port:        8082
+ Start Time:         2019-06-05 12:50:30 (GMT2)
---------------------------------------------------------------------------
+ Server: Apache/2.4.10 (Debian)
+ Cookie PHPSESSID created without the httponly flag
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ OSVDB-3268: /backupweb/: Directory indexing found.
+ Entry '/backupweb/' in robots.txt returned a non-forbidden or redirect HTTP code (200)
+ "robots.txt" contains 1 entry which should be manually viewed.
+ Apache/2.4.10 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Web Server returns a valid response with junk HTTP methods, this may cause false positives.
+ ...
```
Ja je vidět z výsledku skenu, na webu se nachází zaujímavá složka ```/backupweb/```. Otevřeme tedy URL <http://web.hackable.ca:8082/backupweb/> a dostaneme výpis odkesáře ("Index of:")

```
Index of /backupweb

Name  Last modified  Size  Description   
backup.zip  2018-06-11 14:31   583    

Apache/2.4.10 (Debian) Server at web.hackable.ca Port 8082
```

Stáhneme soubor ```backup.zip```, rozbalíme. Obsahem je jediný soubor ```index.php```:
```
<?php
session_start();
if(isset($_POST['passcode'])) {
    if(md5($_POST['passcode']) == '0e039730400727287029396851702161') {
        $_SESSION['loggedin'] = true;
    }
    else {
        echo "INCORRECT PASSWORD";
    }
}
?>
<html>
<head>
<title>Server status Admin</title>
</head>
<body>
<h1>Server Status admin</h1>
<?php if(!$_SESSION['loggedin']) { ?>
    <p>Enter access code to login</p>
    <form method="post">
    <input type="text" name="passcode" placeholder="Passcode">
    <button name="submit">Login</button>    
    </form>
<?php } else { ?>
    <p>Server LOGAN </p>
    <p>Server SQL </p>
    <p>Server SPOCK </p>
    
<?php  $flag = file_get_contents("flag.txt");
    echo $flag;
} ?>
</body>
</html>

```

PHP skript obsahuje MD5 hash ```0e039730400727287029396851702161```, který lze velmi snadno cracknout (např. online na <https://www.md5online.org/md5-decrypt.html>) a výsledkem je heslo ```15529865418```.

Toto heslo použijeme na úvodní stránce této challenge, tím se dostaneme přes autentizaci a dostáváme se na stránku: 

```
Server Status admin

Server LOGAN

Server SQL

Server SPOCK

102 108 97 103 123 112 104 112 95 49 115 95 117 110 115 51 99 117 114 101 125
```

Řetězec čísel vypadá jasně podezřele a jelikož jde pouze o čísla, lze vyvodit, že se jedná o ASCII znaky v decimálním formátu. Pro převod na písmena použijeme jeden z mnoha online konvertorů (např. <https://www.branah.com/ascii-converter>). Konverzí dostáváme ```f l a g { p h p _ 1 s _ u n s 3 c u r e }```, stačí vymazat mezery a máme konečný flag.

FLAG: ```flag{php_1s_uns3cure}```
