## 变量
> 变量就是一组文字或者符号等，来取代一些设置或者是遗传保留的数据，例如myname就是ray，那么我读取myname这个变量时，系统就知道是ray

*示意图：*

![0](/img/12Chapter/Capture3.PNG)

## 变量的取用与设置：echo, 变量设置规则, unset
---
### echo
##### 变量查找：
*使用方法：*
echo + $ + <variable>

*使用范例：*
```Shell
ray@HongKongVPS:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

##### 变量设置/修改：
*使用方法：*
<variable>=xxx

*使用范例：*
```Shell
# 设置myname这个变量
ray@HongKongVPS:~$ echo $myname

ray@HongKongVPS:~$ myname=ray
ray@HongKongVPS:~$ echo $myname
ray
```
注意事项：
- 等号两边不能直接接空白符号，比如错误范例：“**myname = VBird”** 或 **“myname=VBird Tsai** ”

- 变量名称只能是英文或者数字，但开头不能是数字

- 变量内容如果需要有**空白字符**，可以使用**单引号''**括起来，如果还要引用**特殊字符比如$**，则需要使用**双引号""**，不然会被当成一般字符：
```Shell
ray@HongKongVPS:~$ echo var="lang is $LANG"
var=lang is C.UTF-8
ray@HongKongVPS:~$ echo var='lang is $LANG'
var=lang is $LANG
```

- 可用跳脱字符 \ 将特殊符号（如 [Enter], $, \, 空白字符, '等）变成一般字符

- 使用 **$** 可以将指令的**输出赋值给变量**
```Shell
ray@HongKongVPS:~$ version=$(uname -r)
ray@HongKongVPS:~$ echo $version
6.8.0-1018-azure

# 也可以用到其他命令
ray@HongKongVPS:~$ cd /lib/modules/$(uname -r)
ray@HongKongVPS:/lib/modules/6.8.0-1018-azure$ 

ls -ld $(locate crontab)
```

- 可以在已有的变量扩增变量内容
```Shell
ray@HongKongVPS:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
ray@HongKongVPS:~$ PATH="$PATH":/home/bin
ray@HongKongVPS:~$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/bin
```

- 使用**export + 变量**使其变成环境变量供子程序执行

- 使用**unset + 变量**取消变量的设置

## 环境变量的功能
### env
*使用范例：*
**查看**环境变量和说明：

![0](/img/12Chapter/Capture4.PNG)

### set

使用范例：
观察所有变量(环境变量和自订变量)：

![0](/img/12Chapter/Capture5.PNG)