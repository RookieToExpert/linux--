## 路径与指令搜索顺序
### 执行命令的顺序如下:
1. 直接以**相对/绝对路径**执行命令,比如"/bin/ls"或"./ls"
2. 由**alias**找到该指令
3. 由**bash内置的**(builtin)指令执行
4. 通过 **$PATH** 这个变量顺序**搜索第一个指令**
> 可以通过**type -a command** 查找这个command的先后顺序

## bash的进站与欢迎讯息: /etc/issue, /etc/motd
### /etc/issue
> 除了/etc/issue还有一个/etc/issue.net是提供给telnet远程登陆程序使用的
*issue内代码意义:*

![0](/img/12Chapter/Capture15.PNG)

*使用范例:*
```Shell
ray@CloudFrontend:~$ cat /etc/issue
Ubuntu 22.04.5 LTS \n \l
```