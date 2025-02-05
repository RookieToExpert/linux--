## while do done, until do done(不定循环)

#### while do done
*使用方法：*

![0](/img/14Chatper/Capture11.PNG)

> 当某个condition条件成立，进行循环
#### until do done

*使用方法：*

![0](/img/14Chatper/Capture12.PNG)

> 当某个condition条件达成，终止循环

---
*案例一：使用while写一段只有输入yes才能停止的脚本*
```Shell
#!/bin/bash
# Program:
#       Repeat question until user input correct answer.
# History:
# 2/5/2025      Ray     First release

while [[ "${yn}" != "yes" && "${yn}" != "YES" ]]
do
        read -p "Please input yes/YES to stop this program: " yn
done

echo "OK! You input the correct answer."
```

*执行结果：*
```Shell
ray@HongKongVPS:~/ray_learning_sh_folder$ !.
./yes_to_stop.sh 
Please input yes/YES to stop this program: no
Please input yes/YES to stop this program: YES
OK! You input the correct answer.
```


---
*案例一：使用until写一段只有输入yes才能停止的脚本：*
```Shell
"yes_to_stop-2.sh" 11L, 266B                                                           6,38          All
#!/bin/bash
# Program:
#       Use until do done to achive the same goal.
# History:
# 2/5/2025      Ray     First release
until [[ "${yn}" == "yes" || "${yn}" == "YES" ]]
do
        read -p "Please input yes/YES to stop this program: " yn
done

echo "OK! You input the correct answer."
```

执行结果：
```Shell
ray@HongKongVPS:~/ray_learning_sh_folder$ ./yes_to_stop-2.sh 
Please input yes/YES to stop this program: no
Please input yes/YES to stop this program: yes
OK! You input the correct answer.
```
---
*案例二：使用while写一段从1加到用户指定数字的脚本*
```Shell
#!/bin/bash
# Program:
#       Use loop to calculate "1+2+3+...user defined result" result.
# History:
# 2/5/2025      Ray     First release
s=0
i=0
read -p "please enter a number you would like to add up to  from 1." result
while [[ "${i}" != "${result}"  ]]
do
        i=$(($i+1))
        s=$(($s+$i))
done
echo "The result of '1+2..+$result' is ==> $s"
```

*执行结果：*
```Shell
ray@HongKongVPS:~/ray_learning_sh_folder$ ./cal_1_100.sh 
please enter a number you would like to add up to  from 1.10111
The result of '1+2..+10111' is ==> 51121216
```

## for do done(固定循环)
#### 用法一
*使用方法：*

![0](/img/14Chatper/Capture13.PNG)

---
*案例一：for直接跟字段：*
```Shell
#!/bin/bash
# Program:
#       Using for .... loop to print 3 animals
# History:
# 2/5/2025      Ray     First release
for animal in dog cat elephant
do
    echo "There are ${animal}s.... "
done
```


*案例二：for接一个变量*
```Shell
#!/bin/bash
# Program:
#       Use id, finger command to check system account's information.
# History:
# 2/5/2025      Ray     First release

users=$(cut -d ':' -f1 /etc/passwd)    # 撷取帐号名称
for username in ${users}               # 开始循环进行！
do
        id ${username}
done
```

*执行结果：*
```Shell
ray@HongKongVPS:~/ray_learning_sh_folder$ ./userid.sh 
uid=0(root) gid=0(root) groups=0(root)
uid=1(daemon) gid=1(daemon) groups=1(daemon)
uid=2(bin) gid=2(bin) groups=2(bin)
...
```


*案例三：for接一个数组*
```Shell
#!/bin/bash
# Program:
#       Use ping command to check the network's PC state. 
# History:
# 2/5/2025      Ray     First release

network='10.0.0'
# seq命令的用法是seq number number,会创建从number到number之间的数组
for sitenu in $(seq 1 10) 
#或者使用C风格扩展的{}(Bash内置)
# for sitenu in {1..10}
do
        ping -c 1 -w 1 ${network}.${sitenu} > /dev/null && result=0 || result=1
        if [[ "${result}" == 0  ]];then
                echo "Server ${network}.${sitenu} is UP."
        else
                echo "Server ${network}.${sitenu} is DOWN."
        fi
done
```

*执行结果：*
```Shell
ray@HongKongVPS:~/ray_learning_sh_folder$ ./pingip.sh 
Server 10.0.0.1 is DOWN.
Server 10.0.0.2 is DOWN.
Server 10.0.0.3 is DOWN.
Server 10.0.0.4 is UP.
Server 10.0.0.5 is DOWN.
Server 10.0.0.6 is UP.
Server 10.0.0.7 is DOWN.
Server 10.0.0.8 is DOWN.
Server 10.0.0.9 is DOWN.
Server 10.0.0.10 is DOWN.
```

#### 用法二
使用方法：

![0](/img/14Chatper/Capture14.PNG)

>类似i=1;max=100;i++这种写法

*范例一：重新改写cal_1_100.sh:*
```Shell
#!/bin/bash
# Program:
#       Use loop to calculate "1+2+3+...user defined result" result.
# History:
# 2/5/2025      Ray     First release
read -p "Please input a number, I will count for 1+2+...+your_input: " nu

s=0
for ((i=1; i<=${nu}; i=i+1))
do
    s=$((${s}+${i}))
done
echo "The result of '1+2+3+...+${nu}' is ==> ${s}"
```

范例二：随机选择餐厅
```Shell
#!/bin/bash
# Program:
#       Try do tell you what you may eat.
# History:
# 2/5/2025      Ray     First release
eat[1]="卖当当漢堡包"       # 写下你所收集到的店家！
eat[2]="肯爷爷炸鸡"
eat[3]="彩虹日式便当"
eat[4]="越油越好吃大雅"
eat[5]="想不出吃啥学餐"
eat[6]="太师父便当"
eat[7]="池上便当"
eat[8]="怀念火车便当"
eat[9]="一起吃方便面"
eatnum=9
# ${RANDOM}会产生一个0-32767的随机数字，然后利用数学的模运算的均衡性，向下取整，得到1-9的随机结果
check=$((${RANDOM} * ${eatnum}/32767+1))
echo "You may eat ${eat[${check}]}"
```