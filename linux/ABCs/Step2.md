Linux CentOS Step 2
课程工具下载链接：https://pan.baidu.com/s/1c4g1aIs 密码：x3mn

http://man.linuxde.net/

drwxr-xr-x. 2 root root 4096 11月 19 09:26 test

如果是目录  数字 2 代表的是目录中有几个文件及子文件夹
如果是文件  数字   代表的是硬链接数

ln    file.txt  file1   硬链接
ln -s file.txt  file2   软连接 相当于win里的快捷图标


最后行模式 shift  + ：  1，$ s/nologin(旧内容)/666（新输入）/g

http://man.linuxde.net/

一、grep过滤
	－c：只输出匹配行的计数。
	－i：不区分大小写(只适用于单字符)。
	－h：查询多文件时不显示文件名。
	－l：查询多文件时只输出包含匹配字符的文件名。
	－n：显示匹配行及 行号。
	－s：不显示不存在或无匹配文本的错误信息。
	－v：显示不包含匹配文本的所有行。
	grep -i  --color 'Ro' /etc/passwd  大小写忽略
	# grep -i --color  'Anonymous' passwd -B5 -a  -A5

二、管道符|、追加/覆盖符号  命令未结束符号\    
	1、用竖线表示  | ：
		表示将前一个命令的输出结果传递给后面的命令处理，两边都是命令
		$ cat /etc/passwd | more

		1)、grep：过滤筛选
		$ cat /etc/passwd  |  grep 'root'

		2)、过滤条件
	$ ifconfig | grep 'inet'
	$ ls /dev |  grep 'cdrom'

	5、追加和覆盖
	   1、追加 >>:以追加的方式将命令正确的结果输出到文件或设备中区
		date >>1.txt
		echo '123' >>1.txt
		假如命令错误
		data  2>> 1.txt

	   2、覆盖
		date >1.txt
		假如命令错误
		data 2> 1.txt
		标准输入	0	从键盘获得输入	
		标准输出	1	输出到屏幕
		错误输出	2	输出到屏幕

	4、 \ 表示命令未结束换行继续
	注意： \后面不能任何字符，直接回车
		$ cat /etc/sysconfig/\
		> network-scripts/ifcfg-eth0
	
cat /etc/sysconfig/\
network-scripts/ifcfg-eth0
	

三、正则表达式
	
	sed awk
	1、过滤包含数字的行
	# grep '[0-9]' /etc/passwd
	grep '[0-9][0-9][0-9]' /etc/passwd
	# grep ':[0-9][0-9][0-9]:7' /etc/passwd
	# grep '^r.*n$' /etc/passwd
	“[a-z]”“[0-9]”“[A-Z]”
	[A-Z][0-9][0-9] = B02
	
	【举例】提取ifconfig命令中的IP地址，使用sed命令
	# ifconfig | grep 'inet addr:' | grep -v '127.0.0.1' | sed 's/inet addr://g' | sed 's/Bcast.*//g'


	补充命令
	5、wc(word count)统计命令：统计单词、字符、行数，支持管道符号	  
	   $ wc file.log 
       1  4 19 file.log   行数  单词数  字节数 

	   $  wc -l  /etc/passwd   统计行数

	   $  cat /etc/passwd  |  wc  -l 

	6、df  -l -h 显示当前各个硬盘分区的使用情况

	7、du -h 统计文件或目录的大小
	

四、关闭防火墙和安全子系统 （Hadoop HBase这样的分布式集群应用需要）
	--》在联机应用（分布式）中，一般会关闭防火墙。防火墙默认情况下，出于安全考虑会限制一些应用的网络访问（比如rpc通信端口），为了保证多机通信的稳定，可以选择关闭防火墙
1.关闭防火墙并且不开机启动
	1).关闭Linux 防火墙
	# service iptables status   ##查看防火墙状态
	  iptables: Firewall is not running.
	# service iptables stop     ##关闭防火墙
	
	2).设置不开机启动防火墙
	#  chkconfig iptables off   ##不随机启动

	3).查看防火墙的开机启动设置
	#  chkconfig --list | grep iptables

2.关闭安全子系统

	  # vi /etc/sysconfig/selinux
	  SELINUX=disabled
	  
**这里不用去修改
创建虚拟机的时候一定要选择桌面版
	vi /etc/inittab 
	#   0 - halt (Do NOT set initdefault to this):关机
	#   1 - Single user mode：使用单用户模式
	#   2 - Multiuser, without NFS (The same as 3, if you do not have networking)：使用多用户
	#   3 - Full multiuser mode：完全多用户
	#   4 - unused
	#   5 - X11：图形化
	#   6 - reboot (Do NOT set initdefault to this)：重启
	
	把默认值修改为3，就会开启命令行模式
	
五、sudo 权限
	1、操作对象是系统命令
	2、命令： # visudo 管理员（root）身份执行
			或者# vi /etc/sudoers
	3.配置sudo   # visudo
	## Allow root to run any commands anywhere
		root    ALL=(ALL)       ALL
		hadoop  ALL=(root)     NOPASSWD: ALL

	三个ALL到底是什么意思。
	第一个ALL是指网络中的主机，我们后面把它改成了主机名，它指明hadoop用户可以在此主机上执行后面的命令。
	第二个括号里的ALL是指目标用户，也就是以谁的身份去执行命令。
	最后一个ALL当然就是指命令名了。


	4.使用：在需要root用户操作的时候，在原来命令的前方加sudo
	$ vi /etc/hosts
	"/etc/hosts" [readonly] 3L, 189C
	$ sudo vi /etc/hosts
	
	4、which和whereis命令
			whereis定位可执行文件、源代码文件，定位文件在文件系统中的位置
			which同样也是可以定位文件的位置
			which shutdown
			whereis shutdown
			


	
六、Linux中的压缩命令
	1、gzip，压缩时不保留原文件
	2、gzip，不能压缩目录
	3、不能重命名
		压缩前：1668 11月 19 11:24 passwd
		压缩后：697 11月 19 11:24 passwd.gz
		gzip: test/ is a directory -- ignored
		
	3、bzip2适用于一些内容和数据较大的文件进行压缩
	4、tar包格式压缩
	首先要弄清两个概念：打包和压缩。打包是指将一大堆文件或目录变成一个总的文件；压缩
	则是将一个大的文件通过一些压缩算法变成一个小文件。
		-c：建立新的备份文件
		-x：从备份文件中还原文件；
		-v：显示指令执行过程；
		-z：通过gzip指令处理备份文件；
		-f：指定备份文件；

		打包：#tar -cvf test.tar test/
		注意：-f必须放在选项的最后
		打包完成之后再压缩
		gzip test.tar 
		gzip -c file.txt > fiel/file.gz
		gunzip
		解包：# tar -xvf beifeng100.tar -C /opt/
		结合 tar  和  gzip 命令使用
		-C ：这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项
		.tar.gz格式打包：# tar -cvzf beifeng100.tar.gz beifeng100/
		
		.tar.gz格式解包：# tar -zxvf beifeng100.tar.gz -C /opt/rh/
		解压的目录不在当前目录
		选项：-C 重定向，注意是大写
		

七、Linux中的软件包管理方式
	1、软件包的类型
		-》源码包（脚本）
		-》二进制包（rpm、系统默认包）
				-》经过编译后的（看不到源码）
				-》管理方便：安装、卸载、升级、查看
				-》安装速度快
				-》依赖性
					A->B->C->D.....互相依赖
	挂载光驱：# mount /dev/sr0 /media
			  
	2、rpm管理方式
		-》管理.rpm结尾的包
		-》查询：
				rpm -qa  q表示查询，a表示所有
				-》查询所有已经安装好的包
				-i：显示套件的相关信息；
				-v：显示指令执行过程；
				-h：套件安装时列出标记；

		--安装
		rpm -ivh httpd-devel-2.2.15-26.el6.centos.x86_64.rpm 失败的
		rpm -ivh httpd-manual-2.2.15-26.el6.centos.noarch.rpm 	成功
		--卸载
		rpm -e httpd-manual-2.2.15-26.el6.centos.noarch.rpm 
		error: package httpd-manual-2.2.15-26.el6.centos.noarch.rpm is not installed
		rpm -e httpd-manual-2.2.15-26.el6.centos.noarch
		--查看
		（已经安装）# rpm -qa | grep 'httpd'
					# rpm -qa | grep  jdk
					# rpm -qa | grep mysql
				选项：--nodeps：不检测依赖性，一般建议在试验环境使用
				
	3、yum管理方式
		-》使用的前提条件，需要连接到网络
		-》查询：
				# yum list查询已经安装好的包
				# yum list | grep 'httpd'
		-》安装：
				选项：install ，选项：-y  直接确认		
				# yum -y install httpd-devel.x86_64
		-》卸载：
				选项：remove
				# yum -y remove httpd-devel.x86_64
		-》yum仓库：
				地址：/etc/yum.repos.d/
		gpgkey 校验码
		gpgcheck=1 开启校验，0是关闭校验 
		
		如果你的yum源不能使用，可以网上找一下更换yum源
		
50G
20G

八、磁盘管理
	1、查看当前磁盘使用情况
		df -h
	2、磁盘命令
		# fdisk -l
		brw-rw----  其中b代表的是块设备文件
		sda代表第一块硬盘,s代表接口，d代表disk磁盘
		sda1/2...代表硬盘中的分区
		硬盘接口：
		sata sas-》服务器方面
		scsi ide-》个人电脑方面
		SSD固态
	3、cylinders磁柱-》就是查看分区情况
		-》起始和结束
	4、添加磁盘之前先关机，关闭所有的进程
	5、分区
	# fdisk /dev/sdb
	输入m查看帮助信息
	输入n进行分区
	   e   extended			      -》扩展分区
	   p   primary partition (1-4)-》主分区
	   
			主分区+扩展分区<=4
			必须保证要有一个扩展分区
			2+1或者3+1的模式
			主分区分完格式化之后可以直接使用
			扩展分区分完之后还需要进行逻辑分区才能使用
									
	注意：
	分完区之后按  w  进行信息的保存
	分完区之后建议重启机器，让系统重新加载一次信息

	6、格式化磁盘
		Linux中的文件系统
			ext2、ext3、ext4（centos6）、xfs（centos7）等
		格式化命令：mkfs.ext4 /dev/sdb6
	7、挂载磁盘
		挂载临时命令：mount
		mount /dev/sdb5 /mnt
		挂载点：是访问这个分区的唯一入口，是必须已经存在的
		使用：df -h来验证是否挂载成功
		永久生效的挂载方法：
		# vi /etc/fstab  写入配置文件
		/dev/sdb5   /mnt  ext4    defaults  0 0
	
九、克隆虚拟机  VMware
	1.关闭Linux系统  关机：init 0 ; poweroff ; halt; shutdown  重启：init 6 ; reboot
	2.虚拟机--》管理 --》克隆
		选择完整克隆而非克隆链接

	3.克隆出来的虚拟机 要修改主机名还有ip地址，关键是要修改网卡信息
		
	修改主机名： vi /etc/sysconfig/network
	4.# vi /etc/hosts
	修改主机映射：个人的配置为：
	192.168.195.11 bigdata02

	 # vi /etc/sysconfig/network 
	 修改主机名 个人的配置为：apache(原)--》apche clone（克隆）

	5.网卡的信息（mac地址HWADD）
	 	# vi /etc/udev/rules.d/70-persistent-net.rules
	 	删除原来的网卡信息通过mac地址来区分，删除原来的信息
	 	并将新的网卡的name信息改为eth0
	 
	6.重命名网卡配置名称
	 	# mv /etc/sysconfig/network-scripts/ifcfg-Auto_eth1 /etc/sysconfig/network-scripts/ifcfg-eth0	 	

	 	# vi /etc/sysconfig/network-scripts/ifcfg-eth0
	 	BOOTPROTO=static

	 	# service network restart

	7.修改网卡名称：将Auto_eth1 修改为System eth0 

# PCI device 0x8086:0x100f (e1000)
SUBSYSTEM=="net", ACTION=="add", DRIVERS=="?*", ATTR{address}=="00:0c:29:8f:24:b5", ATTR{type}=="1", KERNEL=="eth*", NAME="eth0"

	8.# reboot   重启虚拟机（因为修改了主机名）
	 同时修改windows中主机映射，增加：
	 C:\Windows\System32\drivers\etc\hosts 
	 192.168.195.101  bigdata02


十、系统管理命令
	1、top -》 查看当前系统的资源和任务，3秒刷新一次
		Swap交换分区-》类似windows中的虚拟内存概念
		-》按q退出浏览的状态
	2、free -》 查看当前系统内存资源的情况
		选项：-m 表示mb
	3、netstat
			-》打印Linux中网络系统的状态信息，可让你得知整个Linux系统的网络情况
			-》-t：表示TCP网络协议，三次握手，更安全
			-》-u：表示UDP网络协议，直接传输数据，传输快，不稳定
			-》-l：表示监听端口，listen
			-》-r：表示路由器，查看网关
			-》-n：表示IP地址和端口号
			-》-a：显示所有socket，包括正在监听的。
	   	第一种用法：
			# netstat -tlun
			查看系统已经开启的监听端口
		第二种用法：
			# netstat -an
			-》a表示all全部的意思
			查看系统已经开启的监听端口以及正在连接的网络程序
		第三种用法：
			# netstat -rn
			# route -n同样也是单独查询路由信息
	
		4、ps命令用于报告当前系统的进程状态
			ps -ef
			kill结束系统进程命令
		jps  
			$ kill -9 2287
			-9表示强制关闭，类似windows中的结束任务
		
十一、Linux shell
	1、shell的概念
	  1）shell是一个命令行解释器，它为用户提供了一个向linux内核发送请求以便运行程序界面的系统级程序。
	  用户可以通过shell来启动、挂起、停止。甚至编写一些程序。
	  2）它还是一个功能强大的编程语言，易编辑，易调试，灵活性强。shell是结核性的脚本语言，在我们
	  shell中可以直接调用linux系统命令。
	外层应用程序 ls
		|
	shell命令解释器  --》转换
		|
		内核
		|
		硬件  --》010101010....机器语言 ASCII
     #! shebang
    如果脚本文件中没有#!这一行，那么它执行时会默认用当前Shell去解释这个脚本(即：$SHELL环境变量）
2、shell执行的原理
    1）在我们计算机语言中，最底层是硬件，硬件是通过内核进行管理的。内核通过管理硬件进行功能的实现，但是在计算机语言里底层硬件只能识别0101...的机器语言，无法识别a,b,c,asc码语言。在这中间就需要一个翻译机制进行编译，就是所谓的shell编辑器，           shell编辑器可以讲外层输入的asc码翻译成计算机能识别的机器语言，所以说内核要通过shell编辑器进行翻译才能对硬件进行管理。
    2）反之，硬件要将输入的请求响应给外层应用，那么我们内核是无法识硬件的机器语言，那么这时shell编辑器也会将硬件处理的机器语言翻译成ascll语言给内核，内核再进行shell编辑器反馈给外层应用。最后就会得到我们想要的结果了
	
3、Linux中支持的shell环境
	[root@java15 ~]# cat /etc/shells
	/bin/sh
	/bin/bash
	/sbin/nologin
	/bin/dash
	/bin/tcsh
	/bin/csh
4、shell脚本的执行方式
	. shell.sh
	sh shell.sh
	./shell.sh (必须要有X权限才可以操作) chmod u+x shell.sh

6、变量
	1）环境变量
			系统环境(全局)：cat /etc/profile
			用户环境变量（局部）：cat .bash_profile ，root修改后，beifeng也是不能使用的
				这两类文件别轻易修改
	2）位置变量
		获取脚本文件时所传入的参数，将传入的参数保存在位置变量中，以便于在脚本中可以使用这些变量。
		$1,$2,$3,$4,$5,$6,$7,$8,$9,${10},${11}.....
		编写位置变量：
		[root@java15 sh]# vi 1.sh
			#!/bin/bash
			echo $2
			echo $3
			echo $1
   
		调用脚本并且传入参数
		[root@java15 sh]# sh 1.sh start stop restart
			stop
			restart
			start
	3）预定义变量
		$0 显示脚本名称 
		$! 进程中的PID号，每一个进程都有一个编号 
		$$ 当前进程的id号
		$# 当前传入shell的参数个数
		$* 整体打印参数
		$@ 逐个显示参数内容
		$？判断表示程序退出的代码 （返回0表示成功，非0表示失败） 
	6、4)自定义变量
		  语法格式：name=[value]
		  注意：
			变量对大小敏感
			等号两边不能有空格
		  定义好之后用（$变量名）调用变量
		  用法：a=12
				echo $a
	5、逻辑符号
			&&：逻辑与
				cmd1&&cmd2:表示前一个执行成功后，才会执行后面的命令
				如：# cat /etc/passwrd && mkdir test（不执行第二个命令）
					# cat /etc/passwd && mkdir test（执行第二个命令）
			||：逻辑或
				cmd1||cmd2:表示前面一个命令执行失败，才会执行后面的命令
				如：# cat /etc/passwrd || mkdir hello
			;没有逻辑   (无逻辑)
				cmd1;cmd2
				顺序执行	
	
	6、shell计算
		# echo $((a+b))
		# echo $[a+b]
		# echo $(($a+$b))
		# echo $[$a+$b]  echo $[a+b]不加$也可以执行
		# expr  $a + $b 注意：加号前后要加空格
	7、内置判断
		数字判断：
			-eq 等于则为真           
    		-ne 不等于则为真             
    		-gt 大于则为真               
    		-ge 大于等于则为真           
    		-lt 小于则为真               
    		-le 小于等于则为真           
                             
		数字：
		 a=11
		 b=12
		 [ $a -eq $b ];echo $?
	
		字符串判断：
		=：字符串内容相同则为真，就是说包含的文本一摸一样。
		!=：字符串内容不同，则为真（!号表示相反的意思）
		-z：字符串内容为空（长度为零）则为真		
	
		测试：
		[ "as" = "aa" ];echo $?
		[ "as" != "aa" ];echo $?
		[ -z $a ];echo $?

	[beifeng@bigdata-03 shell]$ a="I am"
	[beifeng@bigdata-03 shell]$ echo $a
	I am
	[beifeng@bigdata-03 shell]$ b="B am"
	[beifeng@bigdata-03 shell]$ [ $a = $b ];echo $?
	-bash: [: too many arguments
	2
	[beifeng@bigdata-03 shell]$ [ "$a" = "$b" ];echo $?
	1

		3)文件测试
			-e 文件名 ： 如果文件名存在即为真
			-r 文件名 ： 如果文件名存在且可读则为真
			-w 文件名 ： 如果文件名存在且可写则为真
			-x 文件名 :  如果文件名存在且可执行则为真
			 ! : 非
			-a : and 
			-o : or
			[ -e  /etc/passwd -a 2 -eq 2 ];echo $?
			[ -e /etc/passwd ];echo $?
			
			
	7、for循环
		#!/bin/bash

		for i in tom tony leo
		do
				echo $i
		done
		【需求】使用for循环
		#!/bin/sh
		for MONTH in {1..12}  //或者这里也可以写$(seq 1 12)
		do
		mkdir -p /opt/test/$MONTH
		done
		
		【需求】求1到10的和
		#!/bin/bash
		for((i=1;i<=10;i++))
		do
		SUM=$((SUM+i))
		done
		echo $SUM
		【扩展】
		使用延迟(有趣的进度条)：
		#!/bin/bash
		b=''
		for ((i=0;$i<=100;i++))
		do printf "Progress:[%-100s]%d%%\r" $b $i
		sleep 0.1
		b=#$b 
		done 
		echo
		
		
		2）if判断：
		第一种写法：
			if 条件判断
			then
			   command.....
			fi
		eg.
			[root@java15 sh]# vi test05.sh
			#!/bin/bash
			if [ 3 -eq 3 ]
			then
			echo Yes
			fi
			[root@java15 sh]# bash test05.sh
			Yes
		第二种写法：
			if 条件语句
			then 
            		   command ....
			else
					   command ....
			fi
		eg.	
			[root@java15 sh]# vi test06.sh
			#!/bin/bash
			if [ -d /home/jerry/tmp ]
			then
			ls /home/jerry/tmp
			else
			mkdir /home/jerry/tmp
			echo "目录创建成功"
			fi
			# sh test06.sh
			目录创建成功			
		
		第三种写法：
			if 条件语句
			then
			command...
			elif 条件语句
			then 
			command...
			elif 条件语句
			then 
			command...
			....
			else
			command...
			fi
		
		eg.
			#!/bi/sh
			# read socre  and choose level
			# read 相当于Java中的scanner 是一种交互式命令，读取设备的输入
			echo "请输入学生成绩score:"
			read score
			if test $score -ge 90;
			then
					echo level A;
			elif [ $score -ge  80  -a  $score -lt 90 ]
			then 
				 echo "level B";
			elif test  $score -ge  70  -a  $score -lt 80 
			then 
				 echo "level C";
			elif [ $score -ge  60 ] && [ $score -lt 70 ]
			then 
				 echo "level D";
			else
					echo 不及格;
			fi
				
	
	8、while循环
		【需求】求1到10的和
		#!/bin/bash
		i=1
		while [ $i -le 10 ]
		do
		SUM=$((SUM+i))
		i=$[i+1]
		done
		echo $SUM
		【需求】读取文件内容
		#!/bin/bash
		while read -r line
		do
		echo $line
		done < /etc/passwd
		
	控制语句case语句
		语法1：
		case $变量名称 in
		条件1）
			命令序列
			；；
		条件2）
			命令序列
			；；
		条件3）
			命令序列
			；；
		*）
		esac
		
		例如：
		#!/bin/bash
		case $1 in
		top)
			top
			;;
		free)
			free
			;;
		ls)
                        ls /opt/cmz
			;;
		*)
			echo "usages: top|free|df"
		esac

	语法2：
	case $变量名称 in
	条件1|条件2）
  		命令序列
  		；；
	条件3|条件4）
  		命令序列
  		；；
	条件5|条件6）
  		命令序列
  		；；
	*）
	esac

	例如：	#!/bin/bash
			case $1 in
			cat|dog)
				echo "animal"
				;;
			pen|pencil)
				echo "study"
				;;
			*)
				echo "usages: cat|dog|pen|pencil"
			esac

十一、Linux中的定时任务
	1、date
		显示当前系统的时间
		# date -s "2017-11-18 17:03:30"
		# date "+%Y%m%d"
		七天前
		# date -d '7 day ago' '+%Y%m%d%H%M'

		
    2、at命令：定时计划任务
		在特定的时间执行一次后结束
		格式：at now+时间（相对于当前时间）
		--at 时间（绝对时间：在当前时间之后的一个具体时间）
			例如：at now+2minutes
					at 10:26
			步骤：at now+2minutes按回车键键入
					at> mkdir /opt/test
					at> 这时可以
		--按Ctrl+d结束编辑
		--# at -l：查看未执行的定时计划

	3、Crontab计划任务
	-->周期性执行计划任务

	选项使用：
	crontab -l   (list )   #查看目前的计划任务列表
	crontab -r   (remove)  #删除计划任务
	crontab -e   (eidt)    #编辑周期性计划任务

	进程名称是crond
	ps -ef | grep crond $查看此进程是否开启
		
		-》检查服务是否开启
		# service crond status
		# service crond start
		# service crond stop
		# service crond restart
		-》执行身份管理员
		-》注意：凡事只要涉及到服务相关的，都会有开机设置项
		开启# chkconfig crond on
		关闭# chkconfig crond off
		-》执行身份管理员
       
	   cat /etc/crontab
	   
	*		*		*		*		*	  user-name command to be executed
	分		时		日		月		周		命令
	
		/ 表示频率
		- 表示范围
		, 表示指定执行
		
【假设场景】
	每周三的凌晨两点
	0	2	*	*	3	cp /etc/passwd /tmp
	每个月10号和25号凌晨1点5分
	5	1	10,25	*	*	*****
	每10分钟执行一次
	*/10	*	*	*	*	*****
	每天凌晨1点到6点
	0	1-6	*	*	*	*****
	每小时的第5分钟执行一次
	5	*	*	*	*	*****
	每月8号的7:30分执行
	30	7	8	*	*	*****
	每年的6月8号5:30分执行
	30	5	8	6	*	*	******
	每天8到11点的第25分钟执行
	25	8-11	*	*	*	*****
	每小时的第5分钟和15分钟和25分钟执行
	5,15,25 * * * *  *******

		【示例】
				每一分钟执行一次将结果写到一个文件中
				*/1 * * * * date >> /opt/test.txt 
				制定完成建议重启服务
				# service crond restart
				


	
	
	
	
	
	
	
	
			

				
