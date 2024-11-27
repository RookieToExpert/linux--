## XFS文件系统备份指令xfsdump
> 第一次使用完整备份，下一次运行xfsdump后就是差异备份(增量备份)

*指示图：*

![0](/img/10Chapter/Capture4.PNG)

#### xfsdump限制
- 只支持**已挂载**的文件系统
- 必须使用**root权限**才能操作
- **只能**备份**XFS**文件系统
- **只能**使用**xfsrestore来解析**
- 是通过UUID来分辨不同的备份文件，**不能**备份两个**具有相同的UUID的文件系统**

#### 使用方法：

![0](/img/10Chapter/Capture5.PNG)


#### 使用案例：
1. *使用**xfsdump**做首次完整备份：*
```Shell
[root@CentOSTest bin]# xfsdump -l 0 -L boot_all -M boot_all -f /srv/boot.dump /boot
xfsdump: using file dump (drive_simple) strategy
xfsdump: version 3.1.8 (dump format 3.0) - type ^C for status and control
xfsdump: level 0 dump of CentOSTest:/boot                       # 开始备份本机/boot系统
xfsdump: dump date: Wed Nov 27 02:00:21 2024                    # 开始备份的时间
xfsdump: session id: dd662cff-ff8c-4127-9e76-bb9935a0a076       # 这次dump的ID
xfsdump: session label: "boot_all"                              # 简单给予一个名字记忆
xfsdump: ino map phase 1: constructing initial dump list        #开始备份程序
xfsdump: ino map phase 2: skipping (no pruning necessary)
xfsdump: ino map phase 3: skipping (only one dump stream)
xfsdump: ino map construction complete
xfsdump: estimated dump size: 45231680 bytes
xfsdump: creating dump session media file 0 (media 0, file 0)
xfsdump: dumping ino map
xfsdump: dumping directories
xfsdump: dumping non-directory files
xfsdump: ending media file
xfsdump: media file size 44909672 bytes
xfsdump: dump size (non-dir files) : 44698160 bytes
xfsdump: dump complete: 1 seconds elapsed
xfsdump: Dump Summary:
xfsdump:   stream 0 /srv/boot.dump OK (success)
xfsdump: Dump Status: SUCCESS
# 查看产生的dump备份文件
[root@CentOSTest srv]# ll /srv/boot.dump 
-rw-r--r--. 1 root root 44909672 11月 27 02:00 /srv/boot.dump
```
2. *查看**/var/lib/xfsdump/inventory**下产生的而文件：*
```Shell
# 查看使用xfsdump之后才产生的/var/lib/xfsdump/inventory的文件(记录备份相关信息比如文件系统/时间/session ID等等，作为下次备份的参考)
[root@CentOSTest srv]# ll /var/lib/xfsdump/inventory/
总用量 16
-rw-r--r--. 1 root root  312 11月 27 02:00 17909981-50b9-46d5-abe5-32236a7d8281.InvIndex
-rw-r--r--. 1 root root 5080 11月 27 02:00 43b2e96c-43fd-4e92-9e81-280853c3e87e.StObj
-rw-r--r--. 1 root root  576 11月 27 02:00 fstab
```
3. *使用**xfsdump -I**查看目前xfsdump备份过的数据信息：*
```Shell
[root@CentOSTest srv]# xfsdump -I
file system 0:
        fs id:          17909981-50b9-46d5-abe5-32236a7d8281
        session 0:
                mount point:    CentOSTest:/boot
                device:         CentOSTest:/dev/sdb1
                time:           Wed Nov 27 02:00:21 2024
                session label:  "boot_all"
                session id:     dd662cff-ff8c-4127-9e76-bb9935a0a076
                level:          0
                resumed:        NO
                subtree:        NO
                streams:        1
                stream 0:
                        pathname:       /srv/boot.dump
                        start:          ino 132 offset 0
                        end:            ino 786564 offset 0
                        interrupted:    NO
                        media files:    1
                        media file 0:
                                mfile index:    0
                                mfile type:     data
                                mfile size:     44909672
                                mfile start:    ino 132 offset 0
                                mfile end:      ino 786564 offset 0
                                media label:    "boot_all"
                                media id:       be247f9d-d150-4495-bf4c-099efbf2b221
xfsdump: Dump Status: SUCCESS
```
*4. 做使用xfsdump做**增量备份**：*
```Shell
# 创建一个空的10MB文件
[root@CentOSTest boot]# dd if=/dev/zero of=/boot/testing.img bs=1M count=10
10+0 records in
10+0 records out
10485760 bytes (10 MB, 10 MiB) copied, 0.0103375 s, 1.0 GB/s

# 使用xfsdump -l 1参数进行增量备份
[root@CentOSTest boot]# xfsdump -l 1 -L boot_2 -M boot_2 -f /srv/boot.dump1 /boot
xfsdump: using file dump (drive_simple) strategy
xfsdump: version 3.1.8 (dump format 3.0) - type ^C for status and control
xfsdump: level 1 incremental dump of CentOSTest:/boot based on level 0 dump begun Wed Nov 27 02:00:21 2024
xfsdump: dump date: Wed Nov 27 02:35:03 2024
xfsdump: session id: 3d1a5085-75c8-4b9b-86cd-29dd4164ff38
xfsdump: session label: "boot_2"
xfsdump: ino map phase 1: constructing initial dump list
xfsdump: ino map phase 2: pruning unneeded subtrees
xfsdump: ino map phase 3: skipping (only one dump stream)
xfsdump: ino map construction complete
xfsdump: estimated dump size: 10506880 bytes
xfsdump: creating dump session media file 0 (media 0, file 0)
xfsdump: dumping ino map
xfsdump: dumping directories
xfsdump: dumping non-directory files
xfsdump: ending media file
xfsdump: media file size 10510952 bytes
xfsdump: dump size (non-dir files) : 10488576 bytes
xfsdump: dump complete: 0 seconds elapsed
xfsdump: Dump Summary:
xfsdump:   stream 0 /srv/boot.dump1 OK (success)
xfsdump: Dump Status: SUCCESS

# 查看新的增量备份文件大小就是10MB
[root@CentOSTest boot]# ll /srv/boot*
-rw-r--r--. 1 root root 44909672 Nov 27 02:00 /srv/boot.dump
-rw-r--r--. 1 root root 10510952 Nov 27 02:35 /srv/boot.dump1

#使用xfsdump -I也可以看到最新的备份信息
```

## XFS文件系统备份还原指令xfsrestore

#### 使用方法：

![0](/img/10Chapter/Capture6.PNG)


#### 使用案例：
1. *使用完整备份**覆盖到原本/boot目录**：*
> 同名文件会被覆盖，其他系统内新的文件会被保留
```Shell
[ray@CentOSTest ~]$ sudo xfsrestore -f /srv/boot.dump -L boot_all /boot
xfsrestore: using file dump (drive_simple) strategy
xfsrestore: version 3.1.8 (dump format 3.0) - type ^C for status and control
xfsrestore: using online session inventory
xfsrestore: searching media for directory dump
xfsrestore: examining media file 0
xfsrestore: reading directories
xfsrestore: 7 directories and 292 entries processed
xfsrestore: directory post-processing
xfsrestore: WARNING: unable to set secure extended attribute for efi: Operation not supported (95)
xfsrestore: restoring non-directory files
xfsrestore: WARNING: path_to_handle of efi failed:Inappropriate ioctl for device
xfsrestore: WARNING: attempt to set extended attributes (xflags 0x80000000, extsize = 0x0, projid = 0x0) of efi failed: Bad file descriptor
xfsrestore: restore complete: 1 seconds elapsed
xfsrestore: Restore Summary:
xfsrestore:   stream 0 /srv/boot.dump OK (success)
xfsrestore: Restore Status: SUCCESS

# 如果要恢复到其他目录，只需要改掉最后的目录路径
[ray@CentOSTest ~]$ mkdir /tmp/boot
[ray@CentOSTest ~]$ xfsrestore -f /srv/boot.dump -L boot_all /tmp/boot
xfsrestore: using file dump (drive_simple) strategy
xfsrestore: version 3.1.8 (dump format 3.0) - type ^C for status and control
...省略
elapsed
xfsrestore: Restore Summary:
xfsrestore:   stream 0 /srv/boot.dump OK (success)
xfsrestore: Restore Status: SUCCESS
# 查看恢复的目录
[ray@CentOSTest ~]$ ll /tmp/boot/
total 38472
-rw-r--r--. 1 ray ray   187644 Oct 22  2020 config-4.18.0-193.28.1.el8_2.x86_64
drwxr-xr-x. 2 ray ray        6 Nov 18  2020 efi
drwx------. 4 ray ray       65 Nov 18  2020 grub2
-rw-------. 1 ray ray 26368692 Nov 18  2020 initramfs-4.18.0-193.28.1.el8_2.x86_64.img
drwxr-xr-x. 3 ray ray       21 Nov 18  2020 loader
-rw-------. 1 ray ray  3911335 Oct 22  2020 System.map-4.18.0-193.28.1.el8_2.x86_64
-rwxr-xr-x. 1 ray ray  8924296 Oct 22  2020 vmlinuz-4.18.0-193.28.1.el8_2.x86_64
```

2. *使用**xfsrestore -s**恢复某个**特定的文件**：*
> 或者使用xfsrestore -i进入管理员交互模式，便查看备份内容边add需要恢复的文件
```Shell
[ray@CentOSTest ~]$ mkdir /tmp/boot2
[ray@CentOSTest ~]$ sudo xfsrestore -f /srv/boot.dump -L boot_all -s grub2 /tmp/boot2
xfsrestore: using file dump (drive_simple) strategy
xfsrestore: version 3.1.8 (dump format 3.0) - type ^C for status and control
...省略
xfsrestore:   stream 0 /srv/boot.dump OK (success)
xfsrestore: Restore Status: SUCCESS
# 查看boot2是否只有恢复的这一个文件
[ray@CentOSTest ~]$ ll /tmp/boot2
total 0
drwx------. 4 root root 65 Nov 18  2020 grub2
```