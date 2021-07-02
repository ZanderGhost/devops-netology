

# devops-netology

1. Разрежённый файл (англ. sparse file) — файл, в котором последовательности нулевых байтов[1]
   
   заменены на информацию об этих последовательностях (список дыр).



2. Нет не могут. Жесткая ссылка является своего рода синонимом к файлу, при создании 

   жесткой ссылки создается указатель на существующий файл, inode у ссылки и файла будет одинаков.

   В связи с этим если мы назначим права файлу или какой-либо из жестких ссылок на файл

   права и на файл и на все жесткие ссылки на него "изменятся" на назначенные.  
   


3. root@vagrant:/home/vagrant# lsblk

    NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda                    8:0    0   64G  0 disk 

    ├─sda1                 8:1    0  512M  0 part /boot/efi

    ├─sda2                 8:2    0    1K  0 part 
    
    └─sda5                 8:5    0 63.5G  0 part 
    
   ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /

   └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]

    sdb                    8:16   0  2.5G  0 disk 

    sdc                    8:32   0  2.5G  0 disk

4. root@vagrant:/home/vagrant# lsblk
   NAME                 MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    
   sda                    8:0    0   64G  0 disk 
    
         ├─sda1                 8:1    0  512M  0 part /boot/efi
         ├─sda2                 8:2    0    1K  0 part 
         └─sda5                 8:5    0 63.5G  0 part 
            ├─vgvagrant-root   253:0    0 62.6G  0 lvm  /
            └─vgvagrant-swap_1 253:1    0  980M  0 lvm  [SWAP]
   
    sdb                    8:16   0  2.5G  0 disk 
   
        ├─sdb1                 8:17   0    2G  0 part 
        └─sdb2                 8:18   0  511M  0 part 
   
    sdc                    8:32   0  2.5G  0 disk 
   



5. root@vagrant:/home/vagrant# sfdisk -d /dev/sdb | sfdisk /dev/sdc 
Checking that no-one is using this disk right now ... OK


Disk /dev/sdc: 2.51 GiB, 2684354560 bytes, 5242880 sectors

Disk model: VBOX HARDDISK   

Units: sectors of 1 * 512 = 512 bytes

Sector size (logical/physical): 512 bytes / 512 bytes

I/O size (minimum/optimal): 512 bytes / 512 bytes

Disklabel type: dos

Disk identifier: 0x30b2b3ed

Old situation:

    >>> Script header accepted.

    >>> Script header accepted.

    >>> Script header accepted.

    >>> Script header accepted.

    >>> Created a new DOS disklabel with disk identifier 0x6f7838a0.

    /dev/sdc1: Created a new partition 1 of type 'Linux' and of size 2 GiB.
    /dev/sdc2: Created a new partition 2 of type 'Linux' and of size 511 MiB.
    /dev/sdc3: Done.

    New situation:
    Disklabel type: dos
    Disk identifier: 0x6f7838a0

    Device     Boot   Start     End Sectors  Size Id Type
    /dev/sdc1          2048 4196351 4194304    2G 83 Linux
    /dev/sdc2       4196352 5242879 1046528  511M 83 Linux
    
    The partition table has been altered.
    Calling ioctl() to re-read partition table.
    Syncing disks.



6. root@vagrant:/home/vagrant# mdadm --create --verbose /dev/md0 -l 1 -n 2 /dev/sd{b,c}1
        
        mdadm: Note: this array has metadata at the start and
            may not be suitable as a boot device.  If you plan to
            store '/boot' on this device please ensure that
            your boot-loader understands md/v1.x metadata, or use
            --metadata=0.90
        mdadm: size set to 2094080K
        Continue creating array? y
        mdadm: Defaulting to version 1.2 metadata
        mdadm: array /dev/md0 started.



7. root@vagrant:/home/vagrant# mdadm --create --verbose /dev/md1 -l 0 -n 2 /dev/sd{b,c}2
    
    mdadm: chunk size defaults to 512K
    
    mdadm: Defaulting to version 1.2 metadata
    
    mdadm: array /dev/md1 started.

   
root@vagrant:/home/vagrant# lsblk
    
    NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    sda                    8:0    0   64G  0 disk  
    ├─sda1                 8:1    0  512M  0 part  /boot/efi
    ├─sda2                 8:2    0    1K  0 part  
    └─sda5                 8:5    0 63.5G  0 part  
      ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
      └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
    sdb                    8:16   0  2.5G  0 disk  
    ├─sdb1                 8:17   0    2G  0 part  
    │ └─md0                9:0    0    2G  0 raid1 
    └─sdb2                 8:18   0  511M  0 part  
      └─md1                9:1    0 1018M  0 raid0 
    sdc                    8:32   0  2.5G  0 disk  
    ├─sdc1                 8:33   0    2G  0 part  
    │ └─md0                9:0    0    2G  0 raid1 
    └─sdc2                 8:34   0  511M  0 part  
      └─md1                9:1    0 1018M  0 raid0 
   
8. root@vagrant:/home/vagrant# pvcreate /dev/md0 /dev/md1

      Physical volume "/dev/md0" successfully created.
      
    Physical volume "/dev/md1" successfully created.
    
    root@vagrant:/home/vagrant# pvdisplay
   
       --- Physical volume ---
   
          PV Name               /dev/sda5
          VG Name               vgvagrant
          PV Size               <63.50 GiB / not usable 0   
          Allocatable           yes (but full)
          PE Size               4.00 MiB
          Total PE              16255
          Free PE               0
          Allocated PE          16255
          PV UUID               OCbATH-NO0a-4yCv-lVyW-UOYQ-uFJm-DPdN8c
           
          "/dev/md0" is a new physical volume of "<2.00 GiB"
          --- NEW Physical volume ---
          PV Name               /dev/md0
          VG Name               
          PV Size               <2.00 GiB
          Allocatable           NO
          PE Size               0   
          Total PE              0
          Free PE               0
          Allocated PE          0
          PV UUID               We7GVz-Rqkg-KTq6-nynb-2SDK-3Kvx-Bo08HJ
           
          "/dev/md1" is a new physical volume of "1018.00 MiB"
          --- NEW Physical volume ---
          PV Name               /dev/md1
          VG Name               
          PV Size               1018.00 MiB
          Allocatable           NO
          PE Size               0   
          Total PE              0
          Free PE               0
          Allocated PE          0
          PV UUID               HDFxyP-0nho-UHEi-IKJt-eBj5-mYMS-JQwSpy

9. root@vagrant:/home/vagrant# vgcreate vgtest /dev/md0 /dev/md1

      Volume group "vgtest" successfully created
    
    root@vagrant:/home/vagrant# pvdisplay
    
          --- Physical volume ---
          PV Name               /dev/sda5
          VG Name               vgvagrant
          PV Size               <63.50 GiB / not usable 0   
          Allocatable           yes (but full)
          PE Size               4.00 MiB
          Total PE              16255
          Free PE               0
          Allocated PE          16255
          PV UUID               OCbATH-NO0a-4yCv-lVyW-UOYQ-uFJm-DPdN8c
           
          --- Physical volume ---
          PV Name               /dev/md0
          VG Name               vgtest
          PV Size               <2.00 GiB / not usable 0   
          Allocatable           yes 
          PE Size               4.00 MiB
          Total PE              511
          Free PE               511
          Allocated PE          0
          PV UUID               We7GVz-Rqkg-KTq6-nynb-2SDK-3Kvx-Bo08HJ
           
          --- Physical volume ---
          PV Name               /dev/md1
          VG Name               vgtest
          PV Size               1018.00 MiB / not usable 2.00 MiB
          Allocatable           yes 
          PE Size               4.00 MiB
          Total PE              254
          Free PE               254
          Allocated PE          0
          PV UUID               HDFxyP-0nho-UHEi-IKJt-eBj5-mYMS-JQwSpy

10. root@vagrant:/home/vagrant# lvcreate -L100 -nlvtest vgtest /dev/md1

     Logical volume "lvtest" created.

11. root@vagrant:/home/vagrant# mount /dev/vgtest/lvtest /tmp/new

    root@vagrant:/home/vagrant# df -hT

        Filesystem                 Type      Size  Used Avail Use% Mounted on
        udev                       devtmpfs  448M     0  448M   0% /dev
        tmpfs                      tmpfs      99M  700K   98M   1% /run
        /dev/mapper/vgvagrant-root ext4       62G  1.4G   57G   3% /
        tmpfs                      tmpfs     491M     0  491M   0% /dev/shm
        tmpfs                      tmpfs     5.0M     0  5.0M   0% /run/lock
        tmpfs                      tmpfs     491M     0  491M   0% /sys/fs/cgroup
        /dev/sda1                  vfat      511M  4.0K  511M   1% /boot/efi
        tmpfs                      tmpfs      99M     0   99M   0% /run/user/1000
        vagrant                    vboxsf    1.8T  161G  1.7T   9% /vagrant
        /dev/mapper/vgtest-lvtest  ext4       93M   72K   86M   1% /tmp/new

12. root@vagrant:/home/vagrant# wget https://mirror.yandex.ru/ubuntu/ls-lR.gz -O /tmp/new/test.gz


    --2021-07-02 06:05:06--  https://mirror.yandex.ru/ubuntu/ls-lR.gz
    Resolving mirror.yandex.ru (mirror.yandex.ru)... 213.180.204.183, 2a02:6b8::183
    Connecting to mirror.yandex.ru (mirror.yandex.ru)|213.180.204.183|:443... connected.
    HTTP request sent, awaiting response... 200 OK
    Length: 21040246 (20M) [application/octet-stream]
    Saving to: ‘/tmp/new/test.gz’
    
    /tmp/new/test.gz                100%[====================================================>]  20.07M   818KB/s    in 31s     
    
    2021-07-02 06:05:38 (660 KB/s) - ‘/tmp/new/test.gz’ saved [21040246/21040246]

13. root@vagrant:/home/vagrant# lsblk

        NAME                 MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
    
        sda                    8:0    0   64G  0 disk  
            ├─sda1                 8:1    0  512M  0 part  /boot/efi
            ├─sda2                 8:2    0    1K  0 part  
            └─sda5                 8:5    0 63.5G  0 part  
              ├─vgvagrant-root   253:0    0 62.6G  0 lvm   /
              └─vgvagrant-swap_1 253:1    0  980M  0 lvm   [SWAP]
            sdb                    8:16   0  2.5G  0 disk  
            ├─sdb1                 8:17   0    2G  0 part  
            │ └─md0                9:0    0    2G  0 raid1 
            └─sdb2                 8:18   0  511M  0 part  
              └─md1                9:1    0 1018M  0 raid0 
                └─vgtest-lvtest  253:2    0  100M  0 lvm   /tmp/new
            sdc                    8:32   0  2.5G  0 disk  
            ├─sdc1                 8:33   0    2G  0 part  
            │ └─md0                9:0    0    2G  0 raid1 
            └─sdc2                 8:34   0  511M  0 part  
              └─md1                9:1    0 1018M  0 raid0 
                └─vgtest-lvtest  253:2    0  100M  0 lvm   /tmp/new


14. root@vagrant:/home/vagrant# gzip -t /tmp/new/test.gz

    root@vagrant:/home/vagrant# echo $?
    
    0

15. root@vagrant:/home/vagrant# pvmove /dev/md1 /dev/md0
    
        /dev/md1: Moved: 12.00%
        /dev/md1: Moved: 100.00%

16. root@vagrant:/home/vagrant# mdadm --fail /dev/md0 /dev/sdc1

        mdadm: set /dev/sdc1 faulty in /dev/md0

17. root@vagrant:/home/vagrant# dmesg

        [14472.979967] md/raid1:md0: Disk failure on sdc1, disabling device.
                       md/raid1:md0: Operation continuing on 1 devices. 

19. root@vagrant:/home/vagrant# gzip -t /tmp/new/test.gz

    root@vagrant:/home/vagrant# echo $?
    
    0




   
 
