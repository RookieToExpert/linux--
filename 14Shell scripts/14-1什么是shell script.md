## shell script的作用
- 自动化管理
    - 类似早期DOS的批处理文件(.bat)，就是将许多指令汇整在一起
    - 将复杂的动作，比如查询登录文件、追踪流量、监控使用者使用主机状态等，能供轻易one touch就能处理
- 追踪和管理系统的重要工作
    - 早期CentOS 6.x以前的版本，系统的services启动的接口是再/etc/init.d/这个项目下，目录的文件都是scripts

## 指令的攥写和执行
- 指令从上而下，从左而右分析和执行；
- 指令、选项与参数间的多个空白都会被忽略掉；
- 空白行也将被忽略，[tab]会被当成空白键忽略；
- 如果读取到一个Enter符号(CR)，就会尝试开始执行改行的命令；
- 至于如果一行的内容太多，可以使用“[Enter]”来延申至下一行
- shell.sh文件必须要具备可读与可执行(rx)权限：
    - 绝对/相对路径来执行；
    - 将shell.sh放在PATH指定的目录中
    - 以bash程序来执行：通过“bash shell.sh”或"sh shell.sh"


## 写一个简单的shell script打印hello world
```Bash
#!/bin/bash
# Program:
#       This program shows "Hello World!" in your screen.
# History:
# 2025/01/21    Ray     First release
# PATH=/home/ray/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
# export PATH
echo -e "Hello World! \a \n"
exit 0
```

1. 第一行：宣告是一个bash的script
2. script的作用
3. script的版本信息，作者等
4. script正文