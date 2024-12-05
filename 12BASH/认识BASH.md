## 硬件、核心与BASH(Shell)
> 计算机大致需要：应用程序，核心管理和硬件，bash就是存在应用程序和核心管理之间的一层壳，帮助用户/应用程序把指令告诉核心管理
*简单示意图：*

![0](/img/12Chapter/Capture.PNG)

linux默认的shell就是bash，使用者登录的时候就会分配一个shell，登录取得的shell就记录在 **/etc/passwd 当中：**
```Shell
# 最后一个数据就是你登陆取得默认的shell
root@Test-Backup-VM:/tmp# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
...
```

## bash的功能
> linux为什么要把bash当成主要的shell呢，看看它的优点
1. 命令编修能力(**history**)：
    - 记忆使用过的指令
    - 存放在 **~/.bash_history**，存放前一次登录的记录，这一次的记录暂时存在内存中
    - 默认存放1000个记录
2. 命令与文件补全功能(**[tab]按键**)

3. 命名别名设置功能(**alias**)：
    - 使用方式：**alias xx='实际指令'**， 例如 alias lm = 'ls -al'

4. 工作控制、前景背景控制：(**job control, foreground, background**)十六章学习

5. 程序化脚本(**shell scripts**)十二章学习

6. 万用字符(__*__)

## type查看指令是否来源于bash
*使用方法：*

![0](/img/12Chapter/Capture1.PNG)

使用案例：
1. 查询ls这个指令是**否为 bash 内置**：
```Shell
# 未加任何参数，列出 ls 的最主要使用情况
ray@Test-Backup-VM:/tmp$ type ls
ls is aliased to 'ls --color=auto'
# 仅列出 ls 执行时的依据
ray@Test-Backup-VM:/tmp$ type -t ls
alias
# 最先使用 aliase, 还有找到外部指令在 /bin/ls
ray@Test-Backup-VM:/tmp$ type -a ls
ls is aliased to 'ls --color=auto'
ls is /usr/bin/ls
ls is /bin/ls
# cd 是 shell 内置指令
ray@Test-Backup-VM:/tmp$ type cd 
cd is a shell builtin
ray@Test-Backup-VM:/tmp$ 
```

## 指令的下达与快速编辑按钮
### \可以换行继续编写命令(适合长命令)

### 立刻删除指令方法

![0](/img/12Chapter/Capture2.PNG)