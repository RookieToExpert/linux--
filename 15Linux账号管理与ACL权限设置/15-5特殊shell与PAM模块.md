## 特殊的shell,/sbin/nologin
1. 系统账户使用/sbin/nologin(或者不需要获得shell的账户)
2. 尝试用nologin的账户登陆时的提示消息可以去 **/etc/nologin.txt** 修改
    - 如果没有此文件，需要手动创建并且去/sbin/nologin编辑，读取这个txt文件


## PAM
*概念图：*

![0](/img/15Chapter/Capture22.PNG)

1. PAM是一个独立的API存在
2. 程序会将PAM模块功能加入，从而顺利使用PAM进行验证
3. 比如使用passwd时，是利用PAM中pam_cracklib.so模块的功能进行验证

*调用过程：*
1. 使用者开始执行 **/usr/bin/passwd** 这支程序，并输入密码；
2. passwd 调用 **PAM** 模块进行验证；
3. PAM 模块会到 **/etc/pam.d/** 找寻与程序 **（passwd） 同名**的配置文件；
4. 依据 **/etc/pam.d/passwd** 内的设置，引用**相关的 PAM 模块**逐步进行验证分析；
5. 将验证结果 （成功、失败以及其他讯息） 回传给**passwd**这支程序；
6. passwd 这支程序会根据 PAM 回传的结果决定下一个动作 （重新输入新密码或者通过验证！）

其中，关于 **/etc/pam.d/passwd的解读** ：

![0](/img/15Chapter/Capture23.PNG)

- 第一个字段：**Type**
    - **auth** 验证使用者的身份，通常需要密码来检验，后续接的模块是验证使用者身份
    - **account** 大部分在进行authorization，检验使用者是否有使用权限(比如密码过期，则无法登录)
    - **session** 管理使用者这次登录(或者使用此命令)期间，记录使用者登陆或者登出时的信息
    - **passwd** 修改/变更密码
- 第二个字段：**control flag**
    - **required** 成功返回success失败返回failure，**不论成功与失败都会继续**后续验证。
    - **requisite** 失败立刻返回**failure并停止后续验证**。
    - **sufficient** 成功立刻返回**success并停止后续验证**
    - **optional** 只是显示讯息，不用在验证。

*大致流程图：*

![0](/img/15Chapter/Capture24.PNG)

- 第三个字段：PAM模块
    - **/etc/pam.d/\***：每个程序个别的 PAM 配置文件；
    - **/lib64/security/\***：PAM 模块文件的实际放置目录；
    - **/etc/security/\***：其他 PAM 环境的配置文件；
    - **/usr/share/doc/pam-*/**：详细的 PAM 说明文档

login时PAM的验证流程：
1. 验证阶段 （auth）：
    - 经过 **pam_securetty.so** 判断
        - 如果是root 时，参考 **/etc/securetty** 的设置；
        - 经过 **pam_env.so** 设置额外的环境变量；
        - 再通过 **pam_unix.so** 检验密码   
            - 若通过则**回报 login 程序**；
            - 若不通过则继续往下以 **pam_succeed_if.so** 判断：     
                - UID 是否**大于 1000 **，若小于 1000则回报失败，否则再往下：
                    - 以 **pam_deny.so** 拒绝连线。

2. 授权阶段 （account）：
    - 先以 **pam_nologin.so** 判断 **/etc/nologin** 是否存在
        - 若存在则不许一般使用者登陆； 
        - 接下来以 **pam_unix.so** 及 **pam_localuser.so** 进行帐号管理
            - 以**pam_succeed_if.so** 判断 UID 是否小于 1000：
                - 若小于 1000 则不记录登录信息。
                - 最后以 **pam_permit.so** 允许该帐号登陆。

3. 密码阶段 （password）：
    - 先以 **pam_pwquality.so** 设置密码仅能尝试错误 3 次
        - 接下来以 **pam_unix.so** 通过 sha512, shadow 等功能**进行密码检验**
            - 若通过则**回报 login 程序**，
            - 若不通过则以 **pam_deny.so 拒绝登陆**。

4. 会议阶段 （session）：
- 先以 **pam_selinux.so** 暂时关闭 SELinux
- 使用 **pam_limits.so** 设置好使用者能够操作的系统资源； 
- 登陆成功后开始记录相关信息在登录文件中； - 以 **pam_loginuid.so** 规范不同的 **UID 权限**；
- 打开 **pam_selinux.so** 的功能。
- 第四个字段：模块的参数

其他相关log：
1. /etc/security/limits.conf: 通过PAM管理一些系统限制

![0](/img/15Chapter/Capture25.PNG)

2. /var/log/secure：PAM报错log