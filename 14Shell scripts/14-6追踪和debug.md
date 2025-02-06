## sh/bash
使用方法：

![0](/img/14Chatper/Capture15.PNG)

*使用范例1，-n检查语法，没有问题不会有任何output：*
```Shell
ray@HongKongVPS:~/ray_learning_sh_folder$ bash -n hello_2.sh 
ray@HongKongVPS:~/ray_learning_sh_folder$ 
```
*使用范例2，使用-x检查script执行时使用的内容显示出来:*
```Shell
ray@HongKongVPS:~/ray_learning_sh_folder$ bash -x netstat.sh 
+ testfile=/dev/shm/netstat_checking.txt
+ netstat -tuln
++ grep ':80 ' /dev/shm/netstat_checking.txt
+ testing=
+ [[ '' != '' ]]
++ grep ':2222 ' /dev/shm/netstat_checking.txt
+ testing='tcp6       0      0 :::2222                 :::*                    LISTEN     '
+ [[ tcp6       0      0 :::2222                 :::*                    LISTEN      != '' ]]
+ echo 'SSH is running in your system.
```

> 可以去/etc/init.d中参阅大佬们写的script