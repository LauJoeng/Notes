##虚拟机的网络三种形式的说明

1. 桥连接：
Linux可以和其他系统通信，相当于多增加了一台电脑。但是可能会造成ip地址冲突
2. NAT连接：
相当于在自己电脑内创建了一个局域网，与计算机网络上讲的NAT转换通信差不多。不会造成ip冲突
3. 仅主机连接：
这是linux是一个独立的主机，不能访问外网。
 

##CentOS 终端的使用和联网

1）鼠标右键即可看到终端菜单

2）网络配置，点击屏幕上面右侧两个计算机的图片，即虚拟网卡，即可连接网络

##vmtools

vmtools可以在windows系统和centos系统之间执行粘贴命令，还可以设置windows和centos共享文件夹


##远程连接

远程连接必须在Linux系统启用SSHD服务，该服务监听22号端口

远程登录工具:XShell,Putty等

远程ftp文件传输工具：FTPRush,XfTP5等

连接后可能显示乱码，改变编码即可
<hr/>

##vi和vim编辑器

所有Linux系统都会内建vi编辑器，vim是vi的增强版本，具有程序编辑能力，可以主动的以字体颜色辨别语法的正确性，方便程序设计。

###vi和vim的三种常见模式

- 正常模式: 这是默认模式，可以使用快捷键，可以使用上下左右键移动光标，删除字符，复制粘贴等动作
- 插入模式(编辑模式):按下任何一个字母之后会进入编辑模式，一般按i
- 命令行模式:提供了相关指令，完成存盘，替换，离开vim，显示行号等动作（:wq保存退出  :q退出，如果做出修改未保存会提示  :q!强制退出，做出的修改不会保存）

一些常用快捷键：

-  yy:拷贝当前行， 5yy:拷贝当前行向下的5行，粘贴按p
-  dd:删除当前行， 5dd:删除向下的5行
-  在文件中查出找某个单词【命令行下 /关键字，回车，输入n就是查找下一个】查询
- 设置文件的行号，取消文件的行号:[命令行下:set nu和 :set nonu]
- [G] 到达文件最末行，[gg]到达文件最首行，都是在正常模式下执行
- 正常模式下可以按 u 撤销 
- 在命令行模式下先按下某个数字，在按shift+g可以将光标移动到指定行，但是要先:set nu打开行号

##开机、重启和用户登录注销
- shutdown
 - shutdown -h now 立即关机
 - shutdown -h 1 表示1分钟后关机:
 - shutdown -r now : 立即重启
- halt:直接使用，等价关机
- reboot:重启 
- sync ：把内存数据同步到磁盘，其实在关机或重启之前都应该执行这个命令，防止数据丢失
- logout 注销，在图形界面执行该命令无效，只是在远程登录时有效

##用户管理
Linux系统是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，以账号进入系统。Linux用户至少要属于一个组

- 添加用户: useradd [选项] 用户名， 选项：-d 【directory】指定创建的用户所在目录【directory】，默认在/home目录下。  —g 指定用户所在组，未指定系统会创建一个与名字同名的分组放进去

- passwd 用户名  可以为用户指定密码

- 删除用户: userdel 【-r】 用户名，-r可选，如果加了-r表示把用户目录也删除
- 查看用户信息: id	 用户名
- su - 用户名  切换用户，且高级用户切换到低级用户不需要密码，低级用户不能切换大高级用户，返回原来用户输入exit即可
- whoami 查看当前用户
- usermod -g 用户组 用户名 ： 修改用户所在的组

###用户组
类似于角色，系统可以对多个有共性的用户进行统一管理

- groupadd 组名 ： 增加组
- groupdel 组名 ： 删除组

##实用指令

###指令运行级别
有七个运行级别

- （0）关机 halt (Do NOT set initdefault to this)
- （1）单用户Single user mode【找回丢失密码】
- （2）多用户无网络服务 Multiuser, without NFS
- （3）多用户有网络服务 Full multiuser mode
- （4）系统未使用保留给用户unused
- （5）图形界面 X11
- （6）系统重启


常用的是3和5

系统的运行级别配置文件四是 /etc/inittab

用init [0123456]可以指定运行级别

**如何找回root密码**

进入到单用户模式，然后修改root密码，因为单用户模式下，root不需要密码就可以登录，这个操作必须在电脑上操作，不能远程操作

[CentOS 7操作方法](https://blog.csdn.net/myth_hg/article/details/44727647)

###帮助指令
对某个指令不熟悉时，可以使用linux的帮助指令来了解指令的使用方法，

语法:  man【命令或配置文件】     ，   help 【指令】

###文件目录类指令

- pwd：显示当前目录绝对路径
- ls [选项] 查看当前目录或文件信息，常用选项：-a表示显示隐藏文件和文件夹，-l表示以列表形式显示
- cd 目录切换 cd ~表示回到自己的home目录，cd ..表示回到上一级目录
- mkdir 【options】 创建目录 ，-p 创建多级目录，如 mkdir
- rmdir 【options】 删除空目录，-rf强制删除非空目录
- touch 文件名...  ：创建一个空文件 ，后面可以跟多个文件，空格分割
- cp 【options】 source dest 拷贝文件到指定目录，-r表示递归拷贝整个文件夹，没有用-r不会拷贝文件夹里面的东西，在cp前面加 “\”会强制覆盖不会提示
- rm 【options】 删除指定文件或目录，-r表示递归删除整个文件夹，-f强制删除不提示
- mv oldNameFile newNameFile ： 移动文件与目录或重命名
- cat 【options】文件  ，查看文件内容，-n表示显示行号，| more表示分页显示，按空格键翻页，Ctrl+b 翻到上一页，q退出，enter键往下翻一行，
- less 文件名 ： 分屏查看文件内容，它并不是一次将文件加载而是显示需要加载的内容，因此显示大文件时非常有用
- “>” 指令 和 “>>” 指令（> 表示直接覆盖， >> 表示追加）下面一些示例：
 - ls -l > 文件 ：列表的内容写入文件中(覆盖)
 - ls -al >>文件 ： 列表的内容追加到文件的末尾
 - cat 文件1 > 文件2 ： 将文件1的内容覆盖到文件2
 - echo "内容">>文件 将内容写入文件 
- echo指令表示输出内容到控制台，echo [options] [contents]
- head指令：显示文件开头部分内容，默认显示前10行内容， 如head -n 5 ： 查看文件前5行
- tail显示文件尾部内容，前两个用法与head相似，tail -f 文件名 : 实时最终文档的所有更新，经常使用
- ln指令，类似windows里的快捷方式，创建了一个指向其他文件路径，ln -s [sourceDir|sourceFile] [name]
- history : 查看已经执行过的历史命令，也可以执行历史命令，该命令后可以跟一个数字，表示显示最近的几条命令，执行历史命令可以用!+命令编号即可

###时间日期类

- date指令：不带参数显示当前时间，date+%Y显示当前年份，date+%m显示当前月份，依次类推，可以格式化，如：date "+%Y-%m-%d %H:%M:%S",date -s 字符串时间 这个指令可以设置时间
- cal [options] 显示当前月日历，后面跟上年份可以显示该年的全部日历

###搜索查找类

- find指令：从指定目录下递归遍历其子目录，将符合条件的文件或者目录显示在终端，语法为：find [搜素范围] [选项]，常用选项有-name，按名称查找，，如 find /home -name hello.txt表示在/home目录下查找hello.txt文件，文件名可以用通配符*，如hello.*。-user表示按用户查找，-size + 容量表示按文件大小搜索，如find / -size -20M表示从根目录下查找小于20M的的文件，大于用+，小于用 - 
- locate指令，快速定位文件路径，它使用事先建立的系统中所有文件名称及路径的locate数据库实现快速定位。为了查询结果的准确度，必须定期更新locate，语法为locate 文件名，第一运行前需要用updatedb创建locate数据库
- grep指令，过滤查找，语法:grep [options] 查找内容 	源文件   ，此外，用管道符 “|”表示将前一个命令处理的结果输出传递给后面的命令，示例:cat hello.java | grep hello   这个命令的意思是浏览hello.java文件，从hello.java文件中查找出hello这个内容的地方，及筛选。grep指令的选项有如下几个，-n，显示行号，-i忽略大小写

###压缩和解压缩类

- gzip/gunzip : gzip 文件名  这个指令用于压缩文件(只能压缩为.gz)，gunzip 文件名 这个指令用于解压缩文件，gzip指令不会保留原文件，可以同时压缩多个文件，文件名排列写即可
- zip [options] xxx.zip 文件名   (压缩文件和目录)，unzip [options] 文件名  (解压缩)，-r递归压缩，-d【dir】 指定解压后存放的目录
- tar指令，打包指令，可以解压也可以压缩，通过不同参数实现。语法 tar [options] xxx.tar.gz 打包的内容， 选项说明 -c 产生.tar打包文件， -v显示详细信息 ， -f 指定压缩后的文件名 ，-z 打包同时压缩，-x 解包.tar文件，如：tar -zcvf a.tar.gz a1.txt a2.txt 这个指令将a1.txt和a2.txt打包为一个a.tar.gz文件，tar -zcvf myhome.tar.gz /home 压缩/home文件夹，tar -zxvf myhome.tar.gz -c /opt/tmp2 将压缩文件解压缩到指定目录
 
<hr/><br/><br/>
##组管理和权限管理

linux中每个用户必须属于一个组，不能独立于组外。文件有所有者，所在组和其他组的概念

- ls -ahl,查看文件所有者
- chown 用户名 文件名  修改文件所有者（change owner的缩写）
- groupadd 组名  添加组
- chgrp 组名 文件名  ： 修改文件所在组
- usermod -g 组名 用户名 : 改变用户所在组
- id 用户名 ： 查看用户信息

rwx权限:r代表可读，w代表可写，x代表可执行，如果是目录代表可以进入

- chmod 修改文件或目录的权限，可以用+，-，=变更权限，u代表所有者，g代表所在组，o代表其他人，a(all)代表所有人所有组。chmod u=wx，g=rx，o=x 文件目录名 表示给u赋xx权限，g赋xx权限，o赋xx权限。chmod o+w 文件目录名 表示给其他人增加一个 w 权限，chmod u-x表示去掉所有者的 x 权限
- 可以通过数字改变权限，规则：r=4，w=2，x=1，chmod u=rwx，g=rx，o=x 文件目录名等价于chmod 751 文件目录名，即把他们的权值相加
- chown修改文件所有者 chown newowner file 改变文件的所有者，chown newowner：newgroup file 改变用户的所有者和所有组 ，-r如果是目录，则使其下所有子文件或目录递归生效。

##cron任务调度

执行定时任务

crontab [options]     -e编辑crondtab定时任务，-l查询crondtab任务，-r终止任务调度，service crond restart 重启任务调度

##Linux磁盘分区、挂载

lsblk 【-l】 查看磁盘挂载分区情况

案例:给Linux系统增加一个新的硬盘，并且挂载到/home/newdisk

1. 虚拟机添加硬盘
2. 分区 ： fdisk /dev/sdb
3. 格式化 ： mkfs -t ext4 /dev/sdb1
4. 挂载 ： 先创建/home/newdisk 目录，挂载 mount /dev/sdb1 /home/newdisk
5. 设置可以自动挂载(永久挂载，当重启系统，仍然可以挂载到指定目录)，在/etc/fstab 文件可以配置自动挂载，卸载用 umount /dev/sdb1 或 umount /newdisk

###磁盘情况查询

- df [options] 查看磁盘整体情况
- df [options] /目录 查询指定目录占用磁盘的情况,-s 指定目录占用大小汇总，-h 带计量单位，-a 含文件，--max-dep=1 子目录深度，-c 列出明细的同时，增加汇总值

案例:

1. 统计/home文件夹下的文件个数 : ls -l /home | grep "^-" | wc -l
2. 统计/home文件夹下的目录个数 ： ls -l /home | grep "^d" | wc -l
3. 统计/home文件夹下文件的个数，包括子文件夹下的 ls -lr /home | grep "^-" | wc -l
4. 以树状显示目录 可能要先yum安装 tree ， 在用tree指令

##Linux网络配置

可以通过虚拟网络编辑器查看虚拟机的ip和网关

网络环境配置

1. 自动获取，在桌面通过网络连接配置。，缺点是每次获取的ip可能是不同的
2. 指定固定的ip地址，直接修改文件来指定ip，并可以连接外网，编辑 /etc/sysconfig/network-scripts/ifcfg-eth0（准确说是eth0的网卡配置文件，多块网卡有多个配置文件），配置IPADDR,GATEWAY,DNS,以及把onboot设置为yes，把bootproto设置为static，可以使用service network restart指令使配置的网络重启生效

##Linux进程管理

Linux中，每个执行的程序(代码)都称为一个进程，每一个进程都分配一个id号，每一个进程都对应一个父进程，而这个父进程可以复制多个子进程，最大的父进程是init进程，分为前台和后台进程。

ps [options] 指令可以查看系统执行的进程，可以不带参数。-a 显示当前终端的所有进程信息，-u 以用户的格式来显示进程的信息。-x 显示后台进程运行的参数，-e显示所有进程，-f以全格式显示。ps -aux 显示全部进程信息，包括用户名，进程id，cpu占用情况，内存占用情况，虚拟内存占用情况，物理内存占用情况，使用的终端，当前进程状态，s代表休眠，r代表运行中，启动时间，占用cpu的总时间，进程启动的命令行，可以用grep命令过滤查找，如 ps -aux | grep sshd 查看sshd服务。

###终止进程kill和killall

kill [options] 进程号（功能:通过进程号杀死进程）常用选项 ，-9 强制杀死

killall 进程名 (功能:通过进程名称杀死进程，支持通配符)

pstree	[options] 查看进程树，-p 显示进程pid，-u 显示进程所属用户

###服务管理()

服务即后台进程，一般一个服务会监听一个端口

service 服务名 [start|stop|restart|reload|status]

CentOS7.0后不在使用service，而是systemctl

关闭防火墙，service iptables stop 立即生效。可以在windows下通过telnet ip地址 端口测试是否可以访问（需要打开telnet客户端）

chkconfig命令可以给各个运行级别设置自启动/关闭，语法 chkconfig -l | grep xxx 查看服务，chkconfig --level 5 服务名 on/off ： 设置在指定级别该服务是否自启动，设置之后需要重启生效

###动态监控进程

top [选项] 该命令与ps很像，不过它会动态更新信息，类似windows任务管理器。选项 -p 以cpu使用率排序，默认就是此项。-m 以内存的使用率排序，-n 以pid排序，-d 指定刷新间隔时间。显示后可以按k指定要杀死的进程。可以查询指定进程。

###监控网络状态 netstat [options]  -p 显示哪个进程在调用 ，-an 按一定顺序排列输出


##rpm和yum

###rpm全称是 RedHat Package Manager（RedHat包管理工具），类似windows的setup.exe

- rpm包简单查询指令 rpm -qai | grep xx (q代表query ，a代表all，i代表information，还有l可以查看安装了哪些文件，f查看文件是哪个rpm包的)

- 卸载rpm包 : rpm -e rpm包名，如果存在有其他包被其他包引用，会产生错误，可以带一个 --nodeps 参数强制删除
- 安装rpm包，rpm -ivh rpm包全路径名称，i表示install，v表示verbose 提示，h代表hash 进度条


###yum全称是 Yellow dog Updater, Modified，是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器，可以自动处理依赖关系，并一次性安装所有依赖的软件包，使用yum必须联网

- yum list | grep xx 软件列表
- yum install xx 安装软件


##Linux搭建JavaEE开发环境

注意事项：
安装tomcat时为了使外部网络能访问需要开放8080端口，修改配置文件 vim /etc/sysconfig/iptables,重启防火墙

##Shell编程

shell是一个命令行解释器，它为用户提供了一个向Linux内核发送请求以便运行程序的界面系统级程序，用户可以用shell来启动，挂起，停止甚至是编写一些应用程序。

- shell脚本的格式要求 ：脚本要以 #!/bin/bash 开头
- 脚本需要有可执行权限，即x权限

shell代码里的单行注释为 # ，多行注释用 :<<!  ! 包起来	


###shell的变量

shell变量分为系统变量和用户自定义的变量，用set命令可以显示当前所有变量

定义变量

- 定义变量 ： 变量=值
- 撤销变量 ： unset 变量
- 声明静态变量 ： readonly 变量，此变量不能unset
- 注意等号两侧不能有空格
- 变量名称一般用大写，引用变量需要加$
- 将命令执行结果返回赋给变量 A='ls -la' 反引号，运行里面的命令，并把结果返回给变量A，，  A=￥$(ls -la) 等价于反引号
- 设置环境变量
 - export 变量名=变量值(将shell变量输出为环境变量)
 - source 配置文件 （让修改后的配置信息立即生效）
 - echo $变量名 (查询当前环境变量的值)

位置参数(相当于C语言的函数参数)，如执行./myshell.sh 100 200,在执行shell脚本时希望获取到后面两个参数

- $n (n为数字，$0代表命令本身，$1-#9代表1到9个参数，10以上的表示为${10})
- $*(代表命令中的所有参数)
- $@(也代表所有参数，不过把每个参数区分对待)
- $#(代表命令行中参数的个数)

案例，编写一个shell脚本，在脚本中获取到命令行的各个参数信息

	#!/bin/bash
	echo "$0 $1 $2"
	echo "$*"
	echo "参数个数:$#"

执行命令./positionPara.sh 90 93 87,输出

	90 93 87
	90 93 87
	参数个数 :3

预定义变量
- $$(获取当前进程的进程号)
- $!（后台运行的最后一个进程的进程号）
- $?（最后一次执行的命令的返回状态，如果这个变量的值为0，证明上一个命令正确执行，入股非0，证明上一个命令执行不正确）

运算符

- "$((运算符))"或"$[运算符]"
- expr m + n （注意运算符之间必须有空格）
- expr \*,/,% 代表乘 ，除，取余

如

	#!/bin/bash
	RESULT1=$(((2 + 3) * 4))
	echo "result1=$RESULT1"

条件判断

[ condition ] (注意condition前后空格，非空返回真)
