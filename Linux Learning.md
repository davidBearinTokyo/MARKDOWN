# Linux Learning
@[2015.08.20]

## 第五章、首次登陆与在线求助 man page

[本章参考](http://vbird.dic.ksu.edu.tw/linux_basic/0160startlinux_3.php)


	LANG="en": setting the system language

--- 


### man page

代表 | 代表内容
----|--------
1 | 使用者在shell环境中可以操作的命令或可运行文件
5 | 配置文件或者是某些文件的格式
8 | 系统管理员可用的管理命令


### man页面内关键词检索
	/keyword



### 搜寻特定命令/文件的man page说明文件

	man -f man
	[davidBear@localhost share]# man -f date
	date                 (1)  - print or set the system 		  
	date and time
	date                 (1p)  - write the date and time

To run the different man page of 'date'

	man 1 date


### 说明文档最重要的三个资源

	man
	info
	/usr/share/doc/

### 正确的关机方法

观察系统的使用状态：

	who
	netstat -a
	ps -aux




# 第六章、Linux 的文件权限与目录配置

### 1. 使用者与群组
Linux 用户身份与群组记录的文件

root相关信息
		
	/etc/passwd

个人密码信息

	/etc/shadow

组名记录

	/etc/group

### 2. Linux文件权限概念

#### 改变所属群组, chgrp

> - 记得，要被改变的组名必须要在/etc/group文件内存在才行，否则就会显示错误！
> - 当你对一个文件具有w权限时，你可以具有写入/编辑/新增/修改文件的内容的权限， 但并不具备有删除该文件本身的权限！

##### 对于目录文件

> 目录的x代表的是用户能否进入该目录成为工作目录的用途！ 所谓的工作目录(work directory)就是你目前所在的目录啦！

如果没有x权限的话，只能显示该目录的内容，不能进入该目录进行操作

- *.sh ： 脚本或批处理文件 (scripts)，因为批处理文件为使用shell写成的，所以扩展名就编成 .sh ；
- *Z, *.tar, *.tar.gz, *.zip, *.tgz： 经过打包的压缩文件。这是因为压缩软件分别为 gunzip, tar 等等的，由于不同的压缩软件，而取其相关的扩展名啰！



# 3. Linux目录配置

> Filesystem Hierarchy Standard (FHS)

HS针对目录树架构仅定义出三层目录底下应该放置什么数据而已, 

- / (root, 根目录)：与开机系统有关
- /usr (unix software resource)：与软件安装/执行有关
- /var (variable)：与系统运作过程有关 


目录 | 应放置文件内容
---- | ----- | 
/bin | 单人维护模式下还能够被操作的指令
/boot | 开机文件|
/dev | 任何装置与接口设备都是以文件的型态存在于这个目录当中的
/etc | 系统主要的配置文件几乎都放置在这个目录内，例如人员的账号密码文件、 各种服务的启始档等等。一般来说，这个目录下的各文件属性是可以让一般使用者查阅的， 但是只有root有权力修改。FHS建议不要放置可执行文件(binary)在这个目录中喔。
/home | 新增一个一般使用者账号时， 默认的用户家目录都会规范到这里来 （~：代表这个用户的家目录）
/lib | 开机时会用到的函式库， 以及在/bin或/sbin底下的指令会呼叫的函式库而已。
/opt | 第三方协力软件放置的目录
/root  | 系统管理员(root)的家目录。之所以放在这里，是因为如果进入单人维护模式而仅挂载根目录时， 该目录就能够拥有root的家目录，所以我们会希望root的家目录与根目录放置在同一个分割槽中。
/sbin | /sbin:开机过程中需要的文件 /usr/bin/: 服务器软件 /usr/local/bin:机器安装的软件
/srv | 网络服务启动之后，所需要去用的数据目录
/tmp | 临时文件防止场所
/lost+found | 文件系统发生错误时，遗失的片段放置在这个目录
/proc:  | 个目录下的数据都是在内存当中， 所以本身不占任何硬盘空间啊！比较重要的文件例如：/proc/cpuinfo, /proc/dma, /proc/interrupts, /proc/ioports, /proc/net/* 等等。
/sys | 虚拟的文件系统，记录与核心相关的信息

必须和 / 放到一起的目录

	/etc: 配置文件
	/bin: 重要执行档, 开机使用
	/dev: 所需要的装置文件
	/lib: 执行党所需要的函数库
	/sbin: 重要的系统执行文件

--- 

### /usr

> usr: unix software resources （FHS建议所有软件开发者，应该将他们的数据合理的分别放置到这个目录下的次目录，而不要自行建立该软件自己独立的目录。）


| 目录 | 应放置文件内容 | 
|--- | ----- |
| /usr/X11R6 |  X Window System重要数据所放置的目录 |
| /usr/bin/ | 用户可使用指令都放在这里|
| /usr/include/ | 头文档的放置处
| /usr/lib | 各种软件的函数库 | 
|  /usr/local | 下载软件的目录 |
| /usr/sbin/ |  非系统正常运作所需要的系统指令。最常见的就是某些网络服务器软件的**服务指令(daemon)**啰！|
| /usr/share/ | /usr/share/man：联机帮助文件 /usr/share/doc：软件杂项的文件说明 /usr/share/zoneinfo：与时区有关的时区文件 | 
|/usr/src/ | 原始码目录


### /var

>  /var就是在系统运作后才会渐渐占用硬盘容量的目录。/var目录主要针对常态性变动的文件，包括缓存(cache)、登录档(log file)以及某些软件运作所产生的文件， 包括程序文件(lock file, run file)，或者例如MySQL数据库的文件等等。

| 目录 | 应放置文件内容 | 
|--- | ----- |
| /var/cache/ | 缓存文件|
| /var/lib/ | 程序本身执行的过程中，需要使用到的数据文件放置的目录。在此目录下各自的软件应该要有各自的目录。 举例来说，MySQL的数据库放置到/var/lib/mysql/而rpm的数据库则放到/var/lib/rpm去！|
| /var/lock |  |
| /var/log/ | **重要到不行！** 这是登录文件放置的目录！里面比较重要的文件如/var/log/messages, /var/log/wtmp(记录登入者的信息)等。|
| /var/mail|电子邮箱|
| /var/run/ | PID存放目录|
| /var/spool/ | 队列数据 工作排程数据(crontab)，就会被放置到/var/spool/cron/目录中！| 

### 绝对路径与相对路径

- .  ：代表当前的目录，也可以使用 ./ 来表示
- .. ：代表上一层目录，也可以 ../ 来代表

# 第七章、Linux 文件与目录管理

> 在写程序 (shell scripts) 来管理系统的条件下，务必使用绝对路径的写法。 