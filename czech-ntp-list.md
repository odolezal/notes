Seznam českých NTP serverů
====

Stratum 1
----
DNS: **ntp.nic.cz**  
IP: 217.31.202.100  
Síť: AS25192  
Referenční zdroj času (Stratum 0): GPS  
Provozovatel: CZ.NIC, z.s.p.o.  
Umístění: Praha  
Podrobnosti: https://www.nic.cz/page/329/cz.nic-prinese-do-ceskych-pocitacu-presny-cas/  

DNS: **tik.cesnet.cz**    
IP: 195.113.144.201  
Síť: AS2852  
Referenční zdroj času (Stratum 0): GPS  
Provozovatel: CESNET z.s.p.o.  
Umístění: Praha  
Podrobnosti: https://www.cesnet.cz/sluzby/casove-sluzby/  

DNS: **tak.cesnet.cz**  
IP: 195.113.144.238  
Síť: AS2852  
Referenční zdroj času (Stratum 0): GPS  
Provozovatel: CESNET z.s.p.o.  
Umístění: Praha  
Podrobnosti: https://www.cesnet.cz/sluzby/casove-sluzby/  

DNS: **ntp2.ufe.cz**  
DNS aliasy: time.ufe.cz, ntp.ufe.cz  
IP: 147.231.2.6  
Síť: AS2852  
Referenční zdroj času (Stratum 0): atomové hodiny  
Provozovatel: Ústav fotoniky a elektroniky, Akademie věd ČR, v.v.i.  
Umístění: Praha  
Podrobnosti: http://www.ufe.cz/cs/tym/laborator-statniho-etalonu-casu-frekvence  

DNS: **lxn.ujf.cas.cz**  
IP: 147.231.100.11  
Síť: AS2852  
Referenční zdroj času (Stratum 0): GPS  
Provozovatel: Ústav jaderné fyziky AV ČR, v. v. i.  
Umístění: Řež (Praha-východ)  
Podrobnosti: http://ujf.cas.cz/  

DNS: **netopyr.hanacke.net**  
IP: 193.85.174.5   
Síť: AS5588  
Referenční zdroj času (Stratum 0): GPS  
Provozovatel: Hanacke.net  
Umístění: ?  
Podrobnosti: http://netopyr.hanacke.net/  

DNS: **ntpm.fit.vutbr.cz**  
IP: 147.229.9.10  
Síť: AS197451   
Referenční zdroj času (Stratum 0): GPS  
Provozovatel: FIT VUT v Brně  
Umístění: Brno  
Podrobnosti: http://ntpm.fit.vutbr.cz/cgi-bin/main  

Stratum 2
----
DNS: **lx.ujf.cas.cz**  
IP: 147.231.100.5   
Síť: AS2852  
Referenční zdroj času (Stratum 1): lxn.ujf.cas.cz  
Provozovatel: Ústav jaderné fyziky AV ČR, v. v. i.  
Umístění: Řež (Praha-východ)  
Podrobnosti: http://ujf.cas.cz/  

DNS: **ns.muni.cz** 
DNS alias: ntp.muni.cz  
IP: 147.251.4.33    
Síť: AS2852  
Referenční zdroj času (Stratum 1): tak.cesnet.cz  
Provozovatel: Masarykova univerzita  
Umístění: Brno  
Podrobnosti:  https://wiki.ics.muni.cz/dalsi_podpurne_technologie/time_server  

DNS: **nora.ics.muni.cz**  
DNS alias: ntp2.muni.cz  
IP: 147.251.4.37     
Síť: AS2852  
Referenční zdroj času (Stratum 1): tik.cesnet.cz   
Provozovatel: Masarykova univerzita  
Umístění: Brno 
Podrobnosti:  https://wiki.ics.muni.cz/dalsi_podpurne_technologie/time_server  

DNS: **pyrrha.fi.muni.cz**  
DNS alias: time.fi.muni.cz  
IP: 147.251.48.140     
Síť: AS2852  
Referenční zdroj času (Stratum 1): tik.cesnet.cz   
Provozovatel: Fakulta informatiky, Masarykova univerzita  
Umístění: Brno  
Podrobnosti: https://www.fi.muni.cz/tech/unix/time.xhtml  

DNS: **titan.zcu.cz**  
DNS alias: clock1.zcu.cz  
IP: 147.228.57.10  
Síť: AS2852  
Referenční zdroj času (Stratum 1): tik.cesnet.cz   
Provozovatel: Západočeská univerzita v Plzni   
Umístění: Plzeň  
Podrobnosti: http://support.zcu.cz/  
 
DNS: **ori.zcu.cz**  
DNS alias: clock2.zcu.cz  
IP: 147.228.52.11      
Síť: AS2852    
Referenční zdroj času (Stratum 1): ?   
Provozovatel: Západočeská univerzita v Plzni  
Umístění: Plzeň  
Podrobnosti: http://support.zcu.cz/  

DNS: **ntp.feec.vutbr.cz**  
IP: 147.229.9.11    
Síť: AS197451  
Referenční zdroj času (Stratum 1): ntpm.fit.vutbr.cz   
Provozovatel: FEKT/FIT VUT v Brně  
Umístění: Brno  
Podrobnosti: http://www.feec.vutbr.cz/OSIS/site.php  


Poznámky
---
* Všechny uvedené servery jsou veřejně přístupné odkudkoli.
* Některé servery neodpovídají na ping, i když jsou plně funkční.
* U serverů, kde je to uvedeno, je doporučeno používat DNS aliasy.
* Seznam stratum 1 serverů obsahuje všechny servery které se podařilo dohledat z veřejných zdrojů.
* Seznam stratum 2 serverů obsahuje jen vybrané servery velkých organizací. Rozhodně není kompletní. Většina veřejně dostupných NTP serverů je zařazena do NTP poolu.
* Statistiky offsetu pro určitý server lze získat na adrese http://www.pool.ntp.org/scores/<IP nebo DNS>

Aktualizace:
---
* 17.4.2018 - převod na GitHub
* 19.12.2015 - doplněny dva servery VUT
