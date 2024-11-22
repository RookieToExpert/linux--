#### swap内存交换空间
> swap是用磁盘空间来暂时存放内存中的信息

##### 如何创建swap
1. *创建swap分区*：
    ```Shell
    ray@HongKongVPS:~$ sudo gdisk /dev/sda
    GPT fdisk (gdisk) version 1.0.10

    Partition table scan:
    MBR: protective
    BSD: not present
    APM: not present
    GPT: present

    Found valid GPT with protective MBR; using GPT.

    Command (? for help): n
    Partition number (2-128, default 2): 
    First sector (34-2099199, default = 2097153) or {+-}size{KMGTP}: 
    Last sector (2097153-2099199, default = 2099199) or {+-}size{KMGTP}: +10K
    Current type is 8300 (Linux filesystem)
    Hex code or GUID (L to show codes, Enter = 8300): 8200
    Changed type of partition to 'Linux swap'

    Command (? for help): w

    Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
    PARTITIONS!!

    Do you want to proceed? (Y/N): Y
    OK; writing new GUID partition table (GPT) to /dev/sda.
    Warning: The kernel is still using the old partition table.
    The new table will be used at the next reboot or after you
    run partprobe(8) or kpartx(8)
    The operation has completed successfully.
    ```

    *此时就能查看到新的SDA2分区了*：
    ```Shell
    ray@HongKongVPS:~$ sudo partprobe
    ray@HongKongVPS:~$ lsblk
    NAME    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
    sda       8:0    0   30G  0 disk 
    ├─sda1    8:1    0   29G  0 part /
    ├─sda2    8:2    0   10K  0 part 
    ├─sda14   8:14   0    4M  0 part 
    ├─sda15   8:15   0  106M  0 part /boot/efi
    └─sda16 259:0    0  913M  0 part /boot
    ```

2. *开始创建swap格式*：
    ```Shell
    [root@study ~]# mkswap /dev/vda6
    Setting up swapspace version 1, size = 524284 KiB
    no label, UUID=6b17e4ab-9bf9-43d6-88a0-73ab47855f9d
    [root@study ~]# blkid /dev/vda6
    /dev/vda6: UUID="6b17e4ab-9bf9-43d6-88a0-73ab47855f9d" TYPE="swap"
    # 确定格式化成功！且使用 blkid 确实可以抓到这个设备了喔！
    ```
    通过free命令可以查看：
    ```Shell
    [root@study ~]# free
     total        used        free      shared  buff/cache   available
    Mem:        1275140      227244      330124        7804      717772      875536  # 实体内存
    Swap:       1048572      101340      947232                                      # swap 相关
    ```

> 当然也可以用loop将文件分区格式化，并且创建swap