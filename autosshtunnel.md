# AutoSSH Reverse Tunnel Run As Daemon With Public Key Authentication

This "how-to" is using **root** user on client side for simplicity and painless deploy.


## Client behind NAT

* ```sudo su -```

* ```apt-get install autossh```
* ```apt-get install openssh-server```

Generate a key for **root** that autossh will use. Take the defaults and use an **empty passphrase!**

```
sudo -i
ssh-keygen 
```
* Copy your public key to the server.

For example: ```ssh-copy-id some_remote_user@some.public.host```

* Test that you can login and accept the key

```ssh some_remote_user@some.public.host```

* Test creating a reverse tunnel. If you're warned that 2222 isn't free, look for another port

```ssh -N -R 2222:localhost:22 -o ServerAliveInterval=240 -o ServerAliveCountMax=2 -o StrictHostKeyChecking=no some_remote_user@some.public.host```


* Create new ```systemd``` service:

* ```/lib/systemd/system/autossh.service```:

```
[Unit]
Description=autossh
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=root
EnvironmentFile=/etc/default/autossh
ExecStart=
ExecStart=/usr/bin/autossh $SSH_OPTIONS
Restart=always
RestartSec=60

[Install]
WantedBy=multi-user.target
```

* ```ln -s /lib/systemd/system/autossh.service /etc/systemd/system/autossh.service```

* ```vim /etc/default/autossh```:

```
AUTOSSH_POLL=60
AUTOSSH_FIRST_POLL=30
AUTOSSH_GATETIME=0
AUTOSSH_PORT=22000
SSH_OPTIONS="-N -R 2222:localhost:22 -o StrictHostKeyChecking=no some_remote_user@some.host.org -i /root/.ssh/id_rsa"
```

* ```systemctl daemon-reload```

* ```systemctl enable /lib/systemd/system/autossh.service```

* ```systemctl start autossh```

* ```systemctl status autossh```
```
● autossh.service - autossh
   Loaded: loaded (/lib/systemd/system/autossh.service; enabled; vendor preset: enabled)
   Active: active (running) since Tue 2020-12-29 16:01:01 CET; 19s ago
 Main PID: 595 (autossh)
    Tasks: 2 (limit: 881)
   CGroup: /system.slice/autossh.service
           ├─595 /usr/lib/autossh/autossh -N -R 2222:localhost:22 -o StrictHostKeyChecking=no some_remote_user@some.host.org -i /root/.ssh/id_rsa
           └─598 /usr/bin/ssh -L 22000:127.0.0.1:22000 -R 22000:127.0.0.1:22001 -N -R 2222:localhost:22 -o StrictHostKeyChecking=no -i /root/.ssh/id_rsa some_remote_user@some.host.org
```

## Remote server with public IP address

Refered as **some.host.org**

* ```ssh localhost -p 2222```

or

*  ```ssh ssh -i .ssh/local_private.key localhost -p 2222```

## Source
* https://sites.google.com/a/gattis.org/know/Work-and-Tech/operating-systems-and-applications/unix/ssh/autossh
* https://www.everythingcli.org/ssh-tunnelling-for-fun-and-profit-autossh/
* https://askubuntu.com/questions/87449/how-to-disable-strict-host-key-checking-in-ssh
