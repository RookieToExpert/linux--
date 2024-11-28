## dd
> 不仅可以创建文件，主要还是可以用于备份

*使用方法：*

![0](/img/10Chapter/Capture7.PNG)

*使用案例：*
1. 将 **/etc/passwd** 备份到 **/tmp/passwd.back**中
```Shell
[ray@CentOSTest ~]$ dd if=/etc/passwd of=/tmp/passwd.back
3+1 records in
3+1 records out
1539 bytes (1.5 kB, 1.5 KiB) copied, 0.000227002 s, 6.8 MB/s
[ray@CentOSTest ~]$ ll /etc/passwd /tmp/passwd.back 
-rw-r--r--. 1 root root 1539 Nov  7 11:49 /etc/passwd
-rw-rw-r--. 1 ray  ray  1539 Nov 28 02:51 /tmp/passwd.back
```

2. 将光驱内容，备份下来成为图像挡
```Shell
[root@study ~]# dd if=/dev/sr0 of=/tmp/system.iso
177612+0 records in
177612+0 records out
90937344 Bytes （91 MB） copied, 22.111 s, 4.1 MB/s
```

3. 将image再烧录至USB，并且将其**挂载至/mnt**
```Shell
[root@study ~]# dd if=/tmp/system.iso of=/dev/sda
[root@study ~]# mount /dev/sda /mnt
[root@study ~]# ll /mnt
dr-xr-xr-x. 131 root root 34816 Jun 26 22:14 etc
dr-xr-xr-x.   5 root root  2048 Jun 17 00:20 home
dr-xr-xr-x.   8 root root  4096 Jul  2 18:48 root
```

4. 将整个/boot的文件系统备份下来：
```Shell
[ray@CentOSTest ~]$ df -h /boot/
Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb1       496M   83M  414M  17% /boot
[ray@CentOSTest ~]$ sudo dd if=/dev/sdb1 of=/tmp/sdb1.img
1024000+0 records in
1024000+0 records out
524288000 bytes (524 MB, 500 MiB) copied, 3.97412 s, 132 MB/s
[ray@CentOSTest ~]$ ll -h /tmp/sdb1.img
-rw-r--r--. 1 root root 500M Nov 28 03:02 /tmp/sdb1.img
```
> dd是默认一个一个扇区去读/写




## cpio
> 可以备份任何东西，包括设备文件

*使用方法：*

![0](/img/10Chapter/Capture8.PNG)

*使用案例：*
1. 找出/boot下所有文件，备份到/tmp/boot.cpio
> 要先到/，然后find boot，是因为如果直接find /boot，未来还原的时候，一定会覆盖掉原本的/boot
```Shell
# sudo sh -c只是将sudo作用与整个命令包括管道分割后的命令
[ray@CentOSTest /]$ sudo sh -c ' find boot | cpio -ocvB > /tmp/boot.cpio'
boot
boot/efi
boot/efi/EFI
boot/efi/EFI/BOOT
...省略
# 查看备份文件
[ray@CentOSTest /]$ ll -h /tmp/boot.cpio 
-rw-r--r--. 1 root root 60M Nov 28 07:04 /tmp/boot.cpio
[ray@CentOSTest /]$ file /tmp/boot.cpio 
/tmp/boot.cpio: ASCII cpio archive (SVR4 with no CRC)
```
2. 将文件再/root/目录下解开
```Shell
[ray@CentOSTest ~]$ cpio -idvc < /tmp/boot.cpio 
boot
boot/efi
boot/efi/EFI
boot/efi/EFI/BOOT
boot/efi/EFI/BOOT/BOOTX64.EFI
boot/efi/EFI/BOOT/fbx64.efi
...省略
```
对比备份文件和源文件：

![0](/img/10Chapter/Capture9.PNG)