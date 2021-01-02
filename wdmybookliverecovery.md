Recovery mount filesystemu z WD MyBookLive 3TB
==

**Připojíme disk k počítači. Zkontrolujeme, zda Linux vidí partitions:**

`sudo fdisk -l`

```
Disk /dev/sdb: 2,7 TiB, 3000592982016 bytes, 5860533168 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 33553920 bytes
Disklabel type: gpt
Disk identifier: 47187517-2EFB-4816-ACB4-9271D0AC821E

Zařízení     Start      Konec    Sektory  Size Druh
/dev/sdb1  1032192    5031935    3999744  1,9G Linux RAID
/dev/sdb2  5031936    9031679    3999744  1,9G Linux RAID
/dev/sdb3    30720    1032191    1001472  489M Microsoft basic data
/dev/sdb4  9031680 5860532223 5851500544  2,7T Microsoft basic data

Partition table entries are not in disk order.
```

**Následně moutneme pseudo RAID pole v read only reživu do složky `/mnt/wd`**

` sudo fuseext2 -o ro -o sync_read -o allow_other /dev/sdb4 /mnt/wd`

```
fuse-umfuse-ext2: version:'0.4', fuse_version:'29' [main (fuse-ext2.c:331)]
fuse-umfuse-ext2: enter [do_probe (do_probe.c:30)]
fuse-umfuse-ext2: leave [do_probe (do_probe.c:55)]
fuse-umfuse-ext2: opts.device: /dev/sdb4 [main (fuse-ext2.c:358)]
fuse-umfuse-ext2: opts.mnt_point: /mnt/wd [main (fuse-ext2.c:359)]
fuse-umfuse-ext2: opts.volname:  [main (fuse-ext2.c:360)]
fuse-umfuse-ext2: opts.options: ro,sync_read,allow_other [main (fuse-ext2.c:361)]
fuse-umfuse-ext2: parsed_options: sync_read,allow_other,ro,fsname=/dev/sdb4 [main (fuse-ext2.c:362)]
fuse-umfuse-ext2: mounting read-only [main (fuse-ext2.c:378)]
```

**Na Raspberry Pi, v systému Raspbian není balíček `fuse-ext2` předkompilován, takže musíme zkompilovat :**

Příprava:
```
sudo apt-get install build-essential
sudo apt-get install m4 autoconf automake libtool
sudo apt-get install libfuse-dev e2fslibs-dev
```

Stáhneme zdrojáky: `https://github.com/alperakcan/fuse-ext2/archive/master.zip`

Rozbalíme: `unzip master.zip`

Zkompilujeme:
```
./autogen.sh
./configure
make
sudo make install
```
Hotovo: `sudo fuse-ext2`

```
fuse-ext2 0.0.9 29 - FUSE EXT2FS Driver

Copyright (C) 2008-2015 Alper Akcan <alper.akcan@gmail.com>
Copyright (C) 2009 Renzo Davoli <renzo@cs.unibo.it>

Usage:    fuse-ext2 <device|image_file> <mount_point> [-o option[,...]]

Options:  ro, force, allow_other
          Please see details in the manual.

Example:  fuse-ext2 /dev/sda1 /mnt/sda1

http://github.com/alperakcan/fuse-ext2
```

### Zdroje
- https://john-hunt.com/2013/04/25/recovering-data-from-a-wd-mybook-live-2tb-3tbor-similar/
- https://ubuntu-mate.community/t/cant-locate-fuseext2-on-16-04-for-rpi-3/7329
- https://github.com/alperakcan/fuse-ext2
- https://medium.com/@aallan/adding-an-external-disk-to-a-raspberry-pi-and-sharing-it-over-the-network-5b321efce86a
- https://www.pcmag.com/how-to/how-to-turn-a-raspberry-pi-into-a-nas-for-whole-home-file-sharing
