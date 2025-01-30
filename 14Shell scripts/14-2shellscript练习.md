## 简单示例

1. 存储用户输入的姓名成为变量
```Shell
#!/bin/bash
# Program:
#       User inputs his first name and last name. Programn shows his full name.
# History:
# 2025/01/22    Ray     First release
read -p "Please input your first name: " firstname      # 提示使用者输入
read -p "Please input your last name: " lastname        # 提示使用者输入
echo -e "\nYour full name is: ${firstname} ${lastname}" # 结果由屏幕输出
```

2. 用户输入文件名，自动创建三个空文件以文件名＋今天日期/昨天日期/前天日期的格式
```Shell
#!/bin/bash
# Program:
#    Program creates three files, which named by user's input and date command.
# History:
# 2025/01/22    Ray     First release

# 1\. 让使用者输入文件名称，并取得fileuser这个变量；
echo -e "I will use 'touch' command to create 3 files." # 纯粹显示信息
read -p "Please input your filename: " fileuser         # 提示使用者输入

# 2\. 为了避免使用者随意按enter,利用[变量功能](../Text/index.html#variable_other_re)分析文件名是否有设置？
# 有疑问参考gpt linux中变量替换"-"问题
filename=${fileuser:-"filename"}                #开始判断是否有配置文件名

# 3\. 开始利用 date 指令来取得所需的文件名；
# 有疑问参考gpt linux中var=$(command)专栏
date1=$(date --date='2 days ago' +%Y%m%d)  #前两天的日期
date2=$(date --date='1 days ago' +%Y%m%d)  #前两天的日期
date3=$(date +%Y%m%d)                       #今天的日期
file1=${filename}${date1}               #下面三行在配置文件名
file2=${filename}${date2}
file3=${filename}${date3}

# 4\. 将文件名创建
touch "${file1}"                        #下面三行创建文件
touch "${file2}"
touch "${file3}"                                   
```


3. 数值运算 
- 用户输入两个整数，然后计算它们乘积并输出到屏幕。
```Shell
#!/bin/bash
# Program:
#       User inputs 2 intger numbers: program will cross these two numbers.
# History:
# 2025/01/22    Ray     First release

echo -e "You SHOULD input 2 numbers, I will multiplying them! \n"
read -p "first number:  " firstnu
read -p "second number: " secnu

# 除了使用declare将变量定义成整数后计算，也可以使用$((计算式))，双括号是bash中的计算方式，bash shell仅
支持到整数的数据计算。
total=$((${firstnu}*${secnu}))
echo -e "\nThe result of ${firstnu} x ${secnu} is => ${total}"
```
如果需要计算小数点，需要通过bc帮忙：
```Shell
ray@HongKongVPS:~$ echo "123.12*22.2"|bc
2733.26
```

- 计算pi
```Shell
#!/bin/bash
# Program:
#      User iinput a scale number to calculate pi number. 
# History:
# 2025/01/22    Ray     First release

echo -e "This program will calculate pi value. \n"
echo -e "You should input a float number to calculate pi value.\n"
read -p "The scale number (10~1000) ?" checking
num=${checking:-"10"}
echo -e "Starting calcuate pi value.  Be patient."
time echo "scale=${num}; 4*a(1)" | bc -lq
```

## source, sh, bash, ./ script不同执行方式的差异

- 直接执行script
    - 不论是**绝对路径/相对路径**还是${PATH}，或者**利用bash或者sh**，script都会使用**一个新的bash环境**来执行脚本内的指令
    - 换句话来说，就是会用一个子程序来执行脚本

**概念图：**

![0](/img/14Chatper/Capture.PNG)

- 使用source执行script
    - 脚本会在父程序中执行

**概念图：**

![0](/img/14Chatper/Capture1.PNG)
