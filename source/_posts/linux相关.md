---
title: linux相关
date: 2018-09-16 01:45:36
tags: operation
---
## Linux 命令
![Image text](/images/LinuxDir.png)
```
系统配置
程序安装
PATH
命令
参数
权限
用户
用户组


常用操作

pwd
    print working dir
    显示现在所处的目录

ls
    不带参数就显示当前目录下的所有文件
    程序可以加参数
    -l 显示详细信息
    -h 人性化显示文件尺寸
    -a 显示所有文件， 以 . 开头的文件是隐藏文件
    还可以带一个目录当参数，这样就会显示这个目录
下面两个是等价的
ls -l -h
ls -lh

cd
    cd Desktop
    改变当前目录
    . 代表当前目录
    .. 代表上级目录
    cd 不带参数就回到默认的家目录
    每个用户都有一个家目录，默认在 /home/用户名
    root 用户的家目录是 /root
cp
    复制出一个文件，用法如下
    cp a.txt b.txt
    复制 a.txt 并把新文件取名为 b.txt
    复制目录要加上 -r 参数
    cp -r a b
mkdir
    创建一个目录
    -p 可以一次性创建多层目录
    mkdir -p a/b/c
rmdir
    只能用来删除一个空目录
rm
    这个命令直接删除东西，很危险，一般不要用
    删除文件或者目录
    -f 强制删除
    -r 用来删除目录
mv
    移动文件或者文件夹
    也可以用来改名
    mv a.txt b.txt
    mv b.txt ../
    mv b.txt ../gua.txt
    可以用 mv xx /tmp 的方式来将文件放入临时文件夹
    （/tmp是操作系统提供的临时文件夹，重启会删除里面的所有文件）
cat
    显示文件内容
tac
    反过来显示文件内容
nl
    显示内容并附带行号
more less head tail
    more 可以分屏分批看文件内容
    less 比 more 更高级，可以前后退看文件
    head 可以显示文件的前 10 行
    tail 可以显示文件的后 10 行
    head 和 tail 有一个 -n 参数
    head -n 20 a.gua
touch
    touch a.gua
    如果 a.gua 存在就更新修改时间
    如果 a.gua 不存在就创建文件


目录分布


权限操作
sudo
    用管理员帐户执行程序
    比如安装程序或者修改一些系统配置都需要管理员权限
su
    switch user， 切换用户
    su gua
    su root

文件权限    文件类型 用户 用户组 文件大小  修改日期     文件名
-rw-rw-r--  1       gua gua     10      11/09 20:28 b.gua
drwxrwxr-x  2       gua gua     4096    11/09 20:28 tmp
文件类型    是否可读  是否可写  是否可执行
d           r       w           x
-           r       w           x
三组 rwx 分表代表 所属用户|同组用户|其他用户
rwx 可以用数字表示为 421
于是乎
r-- 就是 4
rw- 就是 6
rwx 就是 7
r-x 就是 5

chown
    改变文件的用户
    chown gua c.gua
    chown gua:gua c.gua
chmod
    改变文件权限
    chmod 666 root.gua
    chmod +x root.gua
    chmod -x tmp


信息查找
file
    显示文件的类型（不是百分之百准确）
uname
    显示操作系统的名字或者其他信息
    uname -r
    uname -a
which
    which pwd
    显示 pwd 的具体路径
whereis
    whereis ls
    显示更全面的信息
whoami
find . -name ""

奇怪符号
~   家目录快捷方式
>   覆盖式重定向
>>  追加重定向
|   管道, 很麻烦 以后说
``  获取命令执行的结果
&   后台执行
    python3 server.py &
    可以用 fg 命令把一个在后台的程序拉到前台来
    可以用 Ctrl-z 来把一个前台的程序放到后台去挂起
()  开新的子进程shell执行(不用掌握这一条, 因为几乎没人用)
命令配置符
\# 代表超级用户
$ 代表普通用户

history
    查看历史命令
grep
    查找
这两个一般配合使用
    history | grep touch

ps
    查看进程, 一般用下面的用法
    ps ax
ps ax | grep python
    查看带 python 字符串的进程

kill 和 killall 杀进程
    用 ps ax 找到进程id (pid)
    kill [pid]
    kill -9 [pid]
    kill -15 [pid]
    killall 是用进程名字来杀进程

后台前台
fg
jobs

查看端口
netstat -ap | grep 6301

快捷键
C-z 挂起到后台
C-c 中断程序

service application restart/stop/start
    服务 重启、停止， 权限大

reboot
    重启
shutdown
    关机
    可以用参数指定时间
halt
    关机
passwd username
    修改密码
```
## Ubuntu主机设置
```
# ===
# 生成 ssh-key
# ===

# Mac 用户直接打开终端输入命令
# Win 用户安装 Github Desktop 离线包后, 打开桌面的 git shell 程序, 在里面输入下面的命令
# 
# 1. 在本机生成 ssh key 公钥私钥
# 注意 下面的 mykey 随便换一个你喜欢的名字, 这是一个标注, 方便你看的
ssh-keygen -C mykey
# 会提示你生成的文件的地址, 并且让你输入密码, 你不要输入密码, 直接回车
# 
# 这样你就得到了一对 ssh-key, 这是用于登录服务器用的
# 默认你会得到两个文件
# id_rsa 是私钥 自己保存 不要给别人看
# id_rsa.pub 是公钥, 是要到处使用的


# ===
# 重建服务器并且配置 ssh-key
# ===
# 
# 去 vultr 的管理界面
# 先删除(Destory)现有的服务器
# 新建服务器的时候, 把刚才才生成的 id_rsa.pub 文件(用 atom/pycharm 可以打开)里面的内容加入到 ssh-key 步骤中
# 这样你就可以不用密码, 自动登录服务器了(windows 用户请看「windows上用bitvise软件使用sshkey登录.pdf」这个文件)


# 如果你不想重建服务器, 配置 ssh-key 的方法如下
# 在服务器把本机生成的 public key 添加到 /root/.ssh/authorized_keys 文件中
# 1 用 root 用户登录到服务器, 创建 .ssh 目录
cd /root
mkdir .ssh
# 2 编辑 authorized_keys 文件, 把刚才生成的 id_rsa.pub 文件里面的内容粘贴进去并保存退出
# 注意, 这里可以粘贴多个 key, 一行一个
nano .ssh/authorized_keys


# 在本机上可用git bash尝试
# ssh root@ipaddress||domain
# 注意 Are you sure you want to continue connecting (yes/no)?
# 要回答 yes

# 本机使用Bitvise， 点击Client key manager
# import， 点击all文件，找到id_rsa私钥，导入后关闭
# initial method: publickey 选择刚刚生成的文件
# 登录成功

# ===
# 服务器初始化配置, 复制这些命令粘贴到服务器终端中执行
# ===
# 
# 安装 配置 打开 ufw 防火墙
apt-get install ufw
ufw allow 22
ufw allow 80
ufw allow 443
ufw default deny incoming
ufw default allow outgoing
ufw status verbose  
ufw enable


# 安装必备软件
apt-get install git python3 python3-pip python3-setuptools supervisor mongodb redis-server zsh 
# 安装 oh-my-zsh 配置(方便你使用命令行的配置)
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
# 安装 gunicorn 
pip3 install gunicorn pymongo

# ===
# 服务器中文编码问题
# ===
# 
# 编辑下面的文件, 不要拼错
nano /etc/environment
# 加入下面的内容, 保存退出
LC_CTYPE="en_US.UTF-8"
LC_ALL="en_US.UTF-8"

===
gunicorn wsgi --bind 0.0.0.0:2000
===

#!/usr/bin/env python3
这是把代码部署到 apache gunicorn nginx 后面的套路

➜  ~ cat /etc/supervisor/conf.d/xx.conf
supervisor监护程序，重启、崩溃后自动重启
[program:todo]
command=/usr/local/bin/gunicorn wsgi --bind 0.0.0.0:2000 --pid /tmp/todo.pid
directory=/root/web13
autostart=true

可以使用
supervisorctl restart todo

```

## CentOS主机设置

```
Centos7使用SSH，禁用密码登录方法:
1、
# 若没安装openssh，则yum install openssh-server
# 开启sshd， sudo systemctl enable sshd
# sudo systemctl start sshd 或者 sudo service sshd start
# 开启防火墙的22端口
# sudo firewall-cmd --zone=public --add-port=22/tcp --permanent
# sudo service firewalld restart
2、
# 编辑/etc/ssh/sshd_config
# 将PasswordAuthentication参数值修改为no： PasswordAuthentication no
# 重启ssh服务：systemctl restart sshd.service

CentOS防火墙：
1、firewalld的基本使用
启动： systemctl start firewalld
关闭： systemctl stop firewalld
查看状态： systemctl status firewalld 
开机禁用  ： systemctl disable firewalld
开机启用  ： systemctl enable firewalld

2、systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed

3、配置firewalld-cmd

查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic
 
那怎么开启一个端口呢
添加
firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
重新载入
firewall-cmd --reload
查看
firewall-cmd --zone= public --query-port=80/tcp
删除
firewall-cmd --zone= public --remove-port=80/tcp --permanent
```