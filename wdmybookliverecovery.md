Recovery mount filesystemu z WD MyBookLive 3TB
==

**Připojíme disk k počítači. Zkontrolujeme, zda Linux vidí partitions:**

`sudo fdisk -l`

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

**Následně moutneme pseudo RAID pole v read only reživu do složky `/mnt/wd`**

` sudo fuseext2 -o ro -o sync_read -o allow_other /dev/sdb4 /mnt/wd`

Výstup:
fuse-umfuse-ext2: version:'0.4', fuse_version:'29' [main (fuse-ext2.c:331)]
fuse-umfuse-ext2: enter [do_probe (do_probe.c:30)]
fuse-umfuse-ext2: leave [do_probe (do_probe.c:55)]
fuse-umfuse-ext2: opts.device: /dev/sdb4 [main (fuse-ext2.c:358)]
fuse-umfuse-ext2: opts.mnt_point: /mnt/wd [main (fuse-ext2.c:359)]
fuse-umfuse-ext2: opts.volname:  [main (fuse-ext2.c:360)]
fuse-umfuse-ext2: opts.options: ro,sync_read,allow_other [main (fuse-ext2.c:361)]
fuse-umfuse-ext2: parsed_options: sync_read,allow_other,ro,fsname=/dev/sdb4 [main (fuse-ext2.c:362)]
fuse-umfuse-ext2: mounting read-only [main (fuse-ext2.c:378)]
