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

- 其中**PS1**为**提示字符设置**，以下为符号意义：

    ![0](/img/12Chapter/Capture6.PNG)

    比如我想换成以下表达方式：
    > [dmtsai@study /home/dmtsai 16:50 #12]$

    *操作方法：*
    ```Shell
    [dmtsai@study ~]$ cd /home
    [dmtsai@study home]$ PS1='[\u@\h \w \A #\#]\$ '
    [dmtsai@study /home 17:02 #85]$
    # 看到了吗？提示字符变了！变的很有趣吧！其中，那个 #85 比较有趣，
    # 如果您再随便输入几次 ls 后，该数字就会增加喔！为啥？上面有说明滴！
    ```

- __$__ 本身也是变量，表示当前**shell的PID**：

    *操作方法：*
    **echo $$** 即可得到当前PI号码

- **?** 表示上个执行指令的回传值：

    操作方法：
    ```Shell
    # 如果代码顺利执行，则会返回0，否则会返回错误代码(非0)
    ray@CloudFrontend:~$ echo $SHEEL

    ray@CloudFrontend:~$ echo $?
    0
    ray@CloudFrontend:~$ 12name=ray
    12name=ray: command not found
    ray@CloudFrontend:~$ echo $?
    127
    ray@CloudFrontend:~$ 
    ```

- **export** 将自定变量变成环境变量供子程序执行

    *父程序和子程序的关系图：*

    ![0](/img/12Chapter/Capture7.PNG)

    > 子程序不会继承父程序的自定变量，所以需要使用export

### locale
> 影响显示结果的语系变量
- 存放所有语系文件位置: **/usr/lib/locale**
- 语系定义位置: **/etc/locale.conf**

### 变量键盘读取、阵列与宣告：read,edclare,array
- **read**
*使用方法*：

    ![0](/img/12Chapter/Capture8.PNG)

    **使用范例**：
    ```Shell
    # 输入read myname后，光标就会等待你键盘输入
    # -p接提示语，-t接时间
    ray@HongKongVPS:~$ read -p "what is your name:" -t 30 myname
    what is your name:rayray
    ray@HongKongVPS:~$ echo $myname
    rayray
    ```

- **declare**
    > 直接用declare和set输出一样

    *使用方法：*

    ![0](/img/12Chapter/Capture9.PNG)

    *使用范例：*
    1. 使用**declare - i** 将variable定义为**integer类型**
    ```Shell
    ray@HongKongVPS:~$ declare -i sum=100+300+200
    ray@HongKongVPS:~$ echo $sum
    600
    ```

    2. 使用**declare - x** 将变量变**成环境变量**
    ```Shell
    ray@HongKongVPS:~$ declare -x sum
    ray@HongKongVPS:~$ export | grep sum
    declare -ix sum="600"
    ```

    3. 使用**declare -r** 将变量变成**只读**变量：
    > 只能通过登出再登陆才能取消只读
    ```Shell
    ray@HongKongVPS:~$ declare -r sum
    ray@HongKongVPS:~$ sum=111
    -bash: sum: readonly variable
    ```

    4. 选项变成 **+加号可以取消操作**，比如**declare +x** 使变量从环境变量回到自订变量
    ```Shell
    ray@HongKongVPS:~$ declare +x sum
    # 可以通过declare -p查看变量类型
    ray@HongKongVPS:~$ declare -p sum
    declare -ir sum="600"
    ```

- **array**
    1. **定义**一个array：
    ```Shell
    ray@HongKongVPS:~$ var=(1 2 3)
    ```

    2. **访问**该array, 必须通过 **{array[]}**的方式访问：
    ```Shell
    ray@HongKongVPS:~$ echo ${var[@]}
    1 2 3
    ray@HongKongVPS:~$ echo ${var[0]}
    1
    ray@HongKongVPS:~$ echo ${var[1]}
    2
    ray@HongKongVPS:~$ echo ${var[2]}
    3
    ```

## 文件系统和程序的限制关系：ulimit
> 比如如果10个人同时登陆了该机器，但是同时打开了100个文件，这样内存就不够用了，这是会就需要使用ulimit来限制

*使用方法：*

![0](/img/12Chapter/Capture10.PNG)

*使用范例：*
1. 列出所有限制(0=unlimited)：
```Shell
ray@HongKongVPS:~$ ulimit -a
real-time non-blocking time  (microseconds, -R) unlimited
core file size              (blocks, -c) 0
data seg size               (kbytes, -d) unlimited
scheduling priority                 (-e) 0
file size                   (blocks, -f) unlimited
pending signals                     (-i) 3544
max locked memory           (kbytes, -l) 114116
max memory size             (kbytes, -m) unlimited
open files                          (-n) 1024
pipe size                (512 bytes, -p) 8
POSIX message queues         (bytes, -q) 819200
real-time priority                  (-r) 0
stack size                  (kbytes, -s) 8192
cpu time                   (seconds, -t) unlimited
max user processes                  (-u) 3544
virtual memory              (kbytes, -v) unlimited
file locks                          (-x) unlimited
```

2. 限制使用者仅能创建10MBytes以下的容量文件
```Shell
ray@HongKongVPS:~$ ulimit -f 10240
# 需要重新登陆才能解开10M的限制
```

## 变量内容的删除、取代与替换

- 变量的**删除**和**取代**：
    1. **#删除最短匹配部分前**的所有内容：
    ```Shell
    ray@HongKongVPS:~$ echo $path
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
    ray@HongKongVPS:~$ echo ${path#/*local/bin:}
    /usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
    ```
    2. **##删除最长匹配部分前**的所有内容：
    ```Shell
    ray@HongKongVPS:~$ echo ${path##/*:}
    /snap/bin
    ```

    3. **%**则是**从后向前删除**，同样 **%%** 是删除匹配最长部分：
    ```Shell
    ray@HongKongVPS:~$ echo ${path%:*bin}
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
    ray@HongKongVPS:~$ echo ${path%%:*bin}
    /usr/local/sbin
    ```

    4. **/** 是用来取代第一个匹配的结果，**//** 则是取代所有：
    ```Shell
    ray@HongKongVPS:~$ echo ${path/sbin/rrrrrrrrr}
    /usr/local/rrrrrrrrr:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
    ray@HongKongVPS:~$ echo ${path//sbin/rrrrrrrrr}
    /usr/local/rrrrrrrrr:/usr/local/bin:/usr/rrrrrrrrr:/usr/bin:/rrrrrrrrr:/bin:/usr/games:/usr/local/games:/snap/bin
    ```

    *示意图：*

    ![0](/img/12Chapter/Capture11.PNG)

- 变量的**替换**:

    > 主要是使用"-"来判断
    
    *示意图:*
    
    ![0](/img/12Chapter/Capture12.PNG)

    未来用到判断变量时,再回来看
