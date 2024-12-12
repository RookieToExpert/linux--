## 路径与指令搜索顺序
### 执行命令的顺序如下:
1. 直接以**相对/绝对路径**执行命令,比如"/bin/ls"或"./ls"
2. 由**alias**找到该指令
3. 由**bash内置的**(builtin)指令执行
4. 通过 **$PATH** 这个变量顺序**搜索第一个指令**
> 可以通过**type -a command** 查找这个command的先后顺序

## bash的进站与欢迎讯息: /etc/issue, /etc/motd(Unbuntu 在/etc/update-motd.d里面)
### /etc/issue
> 除了/etc/issue还有一个/etc/issue.net是提供给telnet远程登陆程序使用的
*issue内代码意义:*

![0](/img/12Chapter/Capture15.PNG)

*使用范例:*
```Shell
ray@CloudFrontend:~$ cat /etc/issue
Ubuntu 22.04.5 LTS \n \l
```
---
## login Shell & non-login shell
> login shell就是需要通过tty1 ~ tty6输入使用者的账号和密码登录，此时的shell被称作login Shell

> non login shell就是比如在shell里面再输入一个shell这种类似的

### login Shell
- **login shell**会读取以下两个配置文件：
    1. **/etc/profile**，属于**系统整体设置**，最好不要修改
        1. **/etc/profile.d/*.sh**:
            - 执行该目录下所有 **.sh结尾文件**
            - 规范了bash操作接口的**颜色、语系、ll、vi、which与ls的命名别名**
            - 如果需要其他**共享命名别名**，在此目录下**创建.sh文件**
        2. **/etc/locale.conf**：
            - 由 **/etc/profile.d/lang.sh**调用进来的
            - 决定bash使用何种语系，最重要的就是**LANG/LC_ALL**的变量
        3. **/usr/share/bash-completion/completions/***：
            - 由 **/etc/profile.d/bash_completion.sh** 载入的
            - **[tab]**就在这里面，还有一些指令选项/参数补齐功能
    2. **~/.bash_profile**或 **~/.bash_login** 或 **~/.profile**，属于**使用者个人设置**
        > bash的login shell只会读取三个文件中的一个，读取的顺序就是上面列的，如果读去前面的任意一个，后面的都不会读取

*login shell的完整读取流程：*

![0](/img/12Chapter/Capture16.PNG)

> 实线的方向是主线流程，虚线的方向则是被调用的配置文件，所以可以看到被最终调用的是~/.bash_profile这个文件

#### source
> 由于/etc/profile和~/.bash_profile都是取得login shell才会读取的文件，所以如果有修改其中配置，需要运行source才能生效
*使用方法：*

![0](/img/12Chapter/Capture17.PNG)

---
### non-login shell
- non-login shell会读取以下配置文件:
    1.**~/.bashrc**(**non-login shell**的bash操作接口环境的**配置文件**)
    2. ~/.bash_history


### 其他相关配置文件
- **/etc/man_db.conf**：
规定了下达man命令时，man page的路径在哪里
- **~/.bash_history**：
历史命令存储在这里
- **~/.bash_logout**：
登出bash后，系统会执行什么命令

## 终端机的环境设置：stty, set
### stty(setting tty终端机的意思)
*使用方法*：

![0](/img/12Chapter/Capture18.PNG)

![0](/img/12Chapter/Capture19.PNG)

参数含义：
- **intr**: 送出一个 interrupt （中断） 的讯号给目前正在 run 的程序 （就是终止啰！）；
- **quit**: 送出一个 quit 的讯号给目前正在 run 的程序；
- **erase**: 向后删除字符，
- **kill**: 删除在目前命令行上的所有文字；
- **eof**: End of file 的意思，代表“结束输入”。
- **start**: 在某个程序停止后，重新启动他的 output
- **stop**: 停止目前屏幕的输出；
- **susp**: 送出一个 terminal stop 的讯号给正在 run 的程序。

使用范例：
```Shell
# 将erase改成 ctrl h
[dmtsai@study ~]$ stty erase ^h
```

### set
*使用方法*：

![0](/img/12Chapter/Capture20.PNG)

使用范例：
1. 显示目前所有的set设置值：
```Shell
ray@HongKongVPS:~$ echo $-
himBHs
```
2. 设置“若使用未定义变量时，显示错误信息”：
```Shell
[dmtsai@study ~]$ set -u
[dmtsai@study ~]$ echo $vbirding
-bash: vbirding: unbound variable
# 取消，则输入set +u即可
```

3. 设置“执行指令前，显示该指令内容”：
```Shell
[dmtsai@study ~]$ set -x
++ printf '\033]0;%s@%s:%s\007' dmtsai study '~'    # 这个是在列出提示字符的控制码！
[dmtsai@study ~]$ echo ${HOME}
+ echo /home/dmtsai
/home/dmtsai
++ printf '\033]0;%s@%s:%s\007' dmtsai study '~'
```

