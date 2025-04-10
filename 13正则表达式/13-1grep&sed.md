## grep的一些进阶选项

*使用方法：*

![0](/img/13Chapter/Capture.PNG)

*使用案例：*
```Shell
# 因为alias默认使用了auto color，所以不需要加
ray@HongKongVPS:~$ sudo dmesg | grep -n 'memory node range' 
89:[    0.143432] Early memory node ranges
# 显示匹配行数前后三行
ray@HongKongVPS:~$ sudo dmesg | grep -n -A3 -B3 'memory node range' 
86-[    0.139678]   Normal   empty
87-[    0.140742]   Device   empty
88-[    0.141816] Movable zone start for each node
89:[    0.143432] Early memory node ranges
90-[    0.144909]   node   0: [mem 0x0000000000001000-0x000000000009ffff]
91-[    0.147349]   node   0: [mem 0x0000000000100000-0x000000003ff40fff]
92-[    0.149675]   node   0: [mem 0x000000003ffff000-0x000000003fffffff]
```


## sed工具(stream editor流编辑器)
*使用方法：*

![0](/img/13Chapter/Capture1.PNG)


*使用案例：*
使用sed将某些特定行数删掉再显示
```Shell
# 将stdin 2，5行删除
ray@HongKongVPS:~$ nl /etc/passwd | sed '2,5d'
     1  root:x:0:0:root:/root:/bin/bash
     6  games:x:5:60:games:/usr/games:/usr/sbin/nologin
     7  man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
     8  lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
     9  mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    10  news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
```

使用sed在特定行数前后插入单词再显示
```Shell
# 往第二行后插入单词
ray@HongKongVPS:~$ nl /etc/passwd | sed '2a drink tea'
     1  root:x:0:0:root:/root:/bin/bash
     2  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
drink tea
# 往第二行前插入单词
ray@HongKongVPS:~$ nl /etc/passwd | sed '2i drink tea'
     1  root:x:0:0:root:/root:/bin/bash
drink tea
     2  daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
```

使用sed将某些特定行数替换成其他文本
```Shell
# 将第二行替代成其他词
ray@HongKongVPS:~$ nl /etc/passwd | sed '2,5c No 2-5 number'
     1  root:x:0:0:root:/root:/bin/bash
No 2-5 number
     6  games:x:5:60:games:/usr/games:/usr/sbin/nologin
     7  man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
```

使用sed只显示指定行数
```Shell
# 抓出指定的行数
ray@HongKongVPS:~$ nl /etc/passwd | sed -n '5,7p'
     5  sync:x:4:65534:sync:/bin:/bin/sync
     6  games:x:5:60:games:/usr/games:/usr/sbin/nologin
     7  man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
```

使用sed将指定文本替换成对应文本再显示
```Shell
# 替换文本
ray@HongKongVPS:~$ /sbin/ifconfig eth0 | grep 'inet '
        inet 10.0.0.6  netmask 255.255.255.0  broadcast 10.0.0.255
ray@HongKongVPS:~$ /sbin/ifconfig eth0 | grep 'inet ' | sed 's/^.*inet //g'
10.0.0.6  netmask 255.255.255.0  broadcast 10.0.0.255
```

使用sed直接将源文件的指定文本替换成对应文本
```Shell
# 直接修改源文件
ray@HongKongVPS:~$ sed -i 's/\.$/\!/g' regular_express.txt 
```
