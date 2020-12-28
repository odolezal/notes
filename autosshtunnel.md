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

```ssh -N -R 2222:localhost:22 -o ServerAliveInterval=240 -o ServerAliveCountMax=2 some_remote_user@some.public.host```


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
SSH_OPTIONS="-N -R 2222:localhost:22 some_remote_user@some.host.org -i /root/.ssh/id_rsa"
```

* ```systemctl daemon-reload```

* ```systemctl enable /lib/systemd/system/autossh.service```

## Remote server with public IP address

* ```ssh localhost -p 2222```

or

*  ```ssh ssh -i .ssh/local_private.key localhost -p 2222```

## Source
* https://sites.google.com/a/gattis.org/know/Work-and-Tech/operating-systems-and-applications/unix/ssh/autossh
