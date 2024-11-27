#### Nano文本编辑器
> - [ctrl]-G：取得线上说明（help），很有用的！
> - [ctrl]-X：离开naon软件，若有修改过文件会提示是否需要储存喔！
> - [ctrl]-O：储存盘案，若你有权限的话就能够储存盘案了；
> - [ctrl]-R：从其他文件读入数据，可以将某个文件的内容贴在本文件中；
> - [ctrl]-W：搜寻字串，这个也是很有帮助的指令喔！
> - [ctrl]-C：说明目前光标所在处的行数与列数等信息；

#### 观察系统使用状态
- __who__：查看谁在线
- __netstat -a__：查看网路连接状态
- __ps -aux__：查看背景执行状态

#### 关机指令：
- sync
- shutdown
  
![0](/img/06Chapter/Capture.PNG)

- half, poweroff
与shutdown作用相似，只需记得shutdown的用法
> 实际上halt, poweroff, reboot, shutdown都是调用systemctl这个命令
> - systemctl reboot # 系统重新开机
> - systemctl poweroff # 系统关机
