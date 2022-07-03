# 1. linux简介

## Q1：请说出几个linux发行版？

一般来说著名的linux系统基本上分两大类：

1.RedHat系列：Redhat、Centos、Fedora等

2.Debian系列：Debian、Ubuntu等

# 2. 分区与挂载

## Q1：什么是分区？

分区是一个物理概念（相对于目录的逻辑概念），在磁盘格式化后需要进行分区。在Linux系统下(其他操作系统也有类似的规定)，磁盘的分区大致可以分为三类，分别为主分区、扩展分区和逻辑分区等等。

## Q2：一个磁盘最多有多少分区？

通常情况下，一个硬盘中最多能够分割四个主分区。因为硬盘中分区表的大小只有64Bytes，而分割一个分区就需要利用16Bytes空间来存储这个分区的相关信息。由于这个分区表大小的限制，硬盘只能够分给为四个主分区。

为了突破这最多四个主分区的限制，Linux系统引入了扩展分区的概念。即管理员可以把其中一个主分区设置为扩展分区(注意只能够使用一个扩展分区)来进行扩充。而在扩展分区下，又可以建立多个逻辑分区。也就是说，扩展分区是无法直接使用的，必须再细分成逻辑分区才可以用来存储数据。通常情况下，逻辑分区的起始位置及结束位置记录在每个逻辑分区的第一个扇区，这也叫做扩展分区表。在扩展分区下，系统管理员可以根据实际情况建立多个逻辑分区，将一个扩展分区划割成多个区域来使用。

## Q3：为什么要分区？分区的好处？

1. 可以把不同资料，分别放入不同分区中管理，降低风险。

2. 大硬盘搜索范围大，效率低

3. 磁盘配合只能对分区做设定

4. /home /var /usr/local经常是单独分区，因为经常会操作，容易产生碎片

## Q4：什么是挂载？

当要使用某个设备时，例如要读取硬盘中的一个格式化好的分区、光盘或软件等设备时，必须先把这些设备对应到某个目录上，而这个目录就称为“挂载点（mount point）”，这样才可以读取这些设备，而这些对应的动作就是“挂载”。 将物理分区细节屏蔽掉。用户只有统一的逻辑概念。所有的东西都是文件。Mount命令可以实现挂载。

## Q5：分区与目录的关系？

1. 任何一个分区都必须挂载到某个目录上。

2. 目录是逻辑上的区分。分区是物理上的区分。

3. 磁盘Linux分区都必须挂载到目录树中的某个具体的目录上才能进行读写操作。

4. 根目录是所有Linux的文件和目录所在的地方，需要挂载上一个磁盘分区。

## Q6：介绍下对根分区的理解？

挂载为/的分区，就叫做根分区（不管它是主分区还是逻辑分区），它从此开始在整个linux系统里具有了特殊的地位，因为整个电脑里的所有硬盘，包括其上的所有其他分区，不管是主分区、逻辑分区，都将以这个“根分区”为主干，开始构造linux大树，并最终成为这颗树上的一个分支或树叶。

根目录：/必须被挂载，否则无法启动

## Q7：有什么分区建议吗？

A：根目录和swap目录必须被挂载

/boot目录推荐被挂载，存储启动信息

/home目录等经常使用的目录建议被挂载，因为会产生碎片，分区后易清理

# 3. 远程登录linux

## Q1：说出几个远程登录linux的软件？

MobaXterm，putty

## Q2：如何配置远程登录？

1. 首先，安装VMware，并配置一个center os虚拟机。

2. 其次，配置center os的【网络适配器】，如果是有线连接，选择【桥接】，如果是无线连接，选择【仅主机模式】

   ![image-20210424102350684](.\images\image-20210424102350684.png)

3. 回到windows系统，输入ipconfig，如果2选择【桥接】，查看有线网络的ip地址，如果2选择【仅主机模式】，查看VMnet01的ip地址。假设是【192.168.168.1】

   ![image-20210424102437017](.\images\image-20210424102437017.png)

4. 回到虚拟机，输入命令【ifconfig】，查看已安装网络设备，记住某个网络设备的名字，此处是ens33

5. 键入命令

   ifconfig ens33 192.168.168.2

   配置虚拟机网卡与windows同一网段（仅最后一段不同）

   ![image-20210424102652493](.\images\image-20210424102652493.png)

   6. 检查是否可以Ping通，是则代表可以从windows连接到linux

      ![image-20210424102748931](.\images\image-20210424102748931.png)

   7. 安装远程连接软件MobaXterm，配置后就可以使用了

      ![image-20210424103052319](.\images\image-20210424103052319.png)

      ![image-20210424103122299](.\images\image-20210424103122299.png)

## Q3. ifconfig修改网卡ip后，重启后为何失效？

linux的通过命令修改的配置都是临时的，保存在内存中的，重启后会失效。

如需永久改变，需要到指定的配置文件中修改。

## Q4：【远程配置踩坑01】ifconfig如何下载？

首次安装Ubuntu后，ifconfig需要通过apt install下载

![image-20210501113236926](.\images\image-20210501113236926.png)

![image-20210501113411853](.\images\image-20210501113411853.png)

直接安装即可

## Q5：【远程配置踩坑02】虚拟机配置SSH

MobaXterm远程连接虚拟机，需保证虚拟机配置了SSH，可通过如下命令确认虚拟机是否配置SSH :

```
// 确认虚拟机是否配置了SSH
ps -e |grep ssh
```

![image-20210501113637891](.\images\image-20210501113637891.png)

上图ssh-agent表示ssh-server还没有启动，首先安装ssh-server

```
// 启动SSH服务
sudo apt-get install openssh-server
```

执行上述命令后，再查看ssh状态：

![image-20210501113902994](.\images\image-20210501113902994.png)

sshd表示ssh-server已经启动了。

## Q6：【远程配置踩坑03】MobaXterm报错：Access Denied

- /etc/ssh/sshd_config 配置问题：

```bash
#编辑此文件
gedit /etc/ssh/sshd_config
```

- \#PermitRootLogin prohibit-password将该行改为PermitRootLogin yes
- 注意去掉前面的# 再重启ssh: /etc/init.d/ssh restart

# 4. linux命令

## Q1：apt-get

APT： Advanced Packaging Tool，是Debian系列linux操作系统推出的软件包管理系统。替代复杂的源码安装方式。

```
// 举例1：安装软件包，自动下载安装依赖包
apt-get install git
apt-get install python2.7
// 举例2：将所有包的来源更新
apt-get update
```

## Q2：cat

cat（英文全拼：concatenate）命令用于连接文件并打印到标准输出设备上。cat只能查看不能编辑。

```
// 语法
cat [-AbeEnstTuv] [--help] [--version] fileName
```

```
// 举例1：查看指定文件
cat /etc/passwd
// 举例2：查看文件且附带行号
cat -n 1.txt
```

## Q3：cd

change directory，切换到指定目录

```
// 语法
cd [directory]
```

```
// 举例1：切换到目录
cd ~/
// 举例2：切换到上级目录
cd ..
```

## Q4：chmod

Linux chmod（英文全拼：change mode）命令是控制用户对文件的权限的命令

```
// 语法
chmod [-cfvR] [--help] [--version] mode file...
u 表示该文件的拥有者，g 表示与该文件的拥有者属于同一个群体(group)者，o 表示其他以外的人，a 表示这三者皆是。
+ 表示增加权限、- 表示取消权限、= 表示唯一设定权限。
r 表示可读取，w 表示可写入，x 表示可执行，X 表示只有当该文件是个子目录或者该文件已经被设定过为可执行。
```

```
// 举例1：给某个文件的所有用户，增加可执行权限
chmod a+x ~/bin/repo
```

## Q5：cp

Linux cp（英文全拼：copy file）命令主要用于复制文件或目录。

```
// 语法
cp [options] source dest
cp [options] source... directory
```

```
// 举例1：复制当前目录下某个文件，为其创建副本
cp avahi-daemon avahi-daemon.bak
// 举例2：cp命令默认是不能复制目录的，通过-r可复制目录
cp -r 123 /tmp
// 举例3：复制多个文档到指定目录
cp 1.txt 2.txt /home
// 举例4：复制多个目录到指定目录
cp /test1 /test2 /test3
```

## Q6：curl

curl 是常用的命令行工具，用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

```
// 语法
curl [option] [url]
```

```
// 举例1：不带任何参数时，默认GET请求，输出服务器打印的内容
curl www.baidu.com
// 举例2：下载repo工具到指定路径
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
```

## Q7：df

Linux df（英文全拼：disk free） 命令用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计。

```
// 语法
df [选项]... [FILE]...
```

```
// -h : 使用人类可读的方式
df -h
```

## Q8：export

Linux export 命令用于设置或显示环境变量。

在 shell 中执行程序时，shell 会提供一组环境变量。export 可新增，修改或删除环境变量，供后续执行的程序使用。export 的效力仅限于该次登陆操作。

```
// 语法
export [-fnp][变量名称]=[变量设置值]
// 参数
-f 　代表[变量名称]中为函数名称。
-n 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。
-p 　列出所有的shell赋予程序的环境变量。
```

```
// 举例1：查看所有shell赋予程序的环境变量，环境变量采用KEY-VALUE管理，且每个VALUE的各个路径使用冒号隔开，无需空格
export -p
```

![image-20210502150758585](.\images\image-20210502150758585.png)

```
// 举例2：添加新的路径到环境变量PATH，可以灵活调整顺序，放在前面的优先搜索。
export PATH=$PATH:/home/cc/path1:/home/cc/path2
export PATH=/home/cc/path1:/home/cc/path2:$PATH
```

## Q9：head

head 命令可用于查看文件的开头部分的内容，有一个常用的参数 **-n** 用于显示行数，默认为 10，即显示 10 行的内容。

```
head [参数] [文件]  
```

```
// 举例1：默认查看前10行的内容
head 1.txt
// 举例2：指定查看前6行的内容
head -n 6 1.txt
```

## Q10：id

Linux id命令用于显示用户的ID，以及所属群组的ID。

id会显示用户以及所属群组的实际与有效ID。若两个ID相同，则仅显示实际ID。若仅指定用户名称，则显示目前用户的ID。

```
id [-gGnru][--help][--version][用户名称]
```

```
// 举例1：查看当前用户名称
id -un
```

![image-20210502152852411](.\images\image-20210502152852411.png)

## Q11：kill

发送指定信号到指定进程

```
// 语法
kill[参数][进程号]
```

```
// 查看信号列表
kill -l
```

## Q12：less

与more类似，用于查看文件

## Q13：ln

**Linux ln命令**用来为文件创件连接，连接类型分为硬连接和符号连接两种，默认的连接类型是硬连接。如果要创建符号连接必须使用"-s"选项。

```
ln(选项)(参数)
```

```
// 举例1：创建硬链接
ln 1.txt /tmp/1_hard.txt
// 举例2：创建软链接
ln -s 1.txt /tmp/1_soft.txt
```

## Q14：ls

Linux ls（英文全拼：list files）命令用于显示指定工作目录下之内容（列出目前工作目录所含之文件及子目录)。

```
// 语法
ls [-alrtAFR] [name...]
```

```
// 举例1：列举文件/目录名称
ls /home
// 举例2：列举所有文件，包含隐藏文件
ls —a /home
ls --all /home
// 举例3：列举文件详细信息（长格式显示）
ls -l /home
// 举例4：人性化显示（文件大小按Kb/Mb等实际大小显示）（需结合-l或-s一起使用）
ls -lh /home
ls --human -l /home
ls --human-readable -l /home
// 举例5：查看目录的详细信息，而非目录下的文件（需结合-l一起使用）
ls -ld /home
// 举例6：不填路径时，默认当前路径
ls
```

![image-20210502153813276](.\images\image-20210502153813276.png)

```
// 补充说明ls -l中的信息：从上图中可以看到一共7列
// 第一列：文件属性
	-表示普通文件
	d表示目录文件
	l表示链接文件
	b表示块设备文件
	c表示字符设备文件
	p表示命令管道文件
	s表示sock文件
// 第二列：文件硬链接数
// 第三列：文件/目录拥有者
// 第四列：文件/目录拥有者所在的组
// 第五列：文件所占用的空间（以字节为单位）（-h可以按Kb/Mb等人性化显示）
// 第六列：文件/目录最近访问（修改）时间
// 第七列：文件/目录名
```

## Q15：mkdir

Linux mkdir（英文全拼：make directory）命令用于创建目录。

```
// 语法
mkdir [-p] dirName
```

```
// 举例1：创建test目录
mkdir test
// 举例2：递归创建目录（当test1，test2不存在时，可创建成功）
mkdir -p test/test1/test2/test3
// 举例3：同时创建多个目录
mkdir test/test1 test/test2 test/test3
```

## Q16：more

Linux more 命令类似 cat ，不过会以一页一页的形式显示，更方便使用者逐页阅读，而最基本的指令就是按空白键（space）就往下一页显示，按 b 键就会往回（back）一页显示，而且还有搜寻字串的功能（与 vi 相似），使用中的说明文件，请按 h 。

```
// 语法
more [-dlfpcsu] [-num] [+/pattern] [+linenum] [fileNames..]

-num 一次显示的行数
-d 提示使用者，在画面下方显示 [Press space to continue, 'q' to quit.] ，如果使用者按错键，则会显示 [Press 'h' for instructions.] 而不是 '哔' 声
-l 取消遇见特殊字元 ^L（送纸字元）时会暂停的功能
-f 计算行数时，以实际上的行数，而非自动换行过后的行数（有些单行字数太长的会被扩展为两行或两行以上）
-p 不以卷动的方式显示每一页，而是先清除萤幕后再显示内容
-c 跟 -p 相似，不同的是先显示内容再清除其他旧资料
-s 当遇到有连续两行以上的空白行，就代换为一行的空白行
-u 不显示下引号 （根据环境变数 TERM 指定的 terminal 而有所不同）
+/pattern 在每个文档显示前搜寻该字串（pattern），然后从该字串之后开始显示
+num 从第 num 行开始显示
fileNames 欲显示内容的文档，可为复数个数

注：[-dlfpcsu] [-num] [+/pattern] [+linenum]都是属于选项，没有顺序要求
```

```
// 举例1：查看某个文件
more 1.txt
// 举例2：查看某个文件，限制一次显示20行
more -20 1.txt
// 举例3：从第20行开始查看某个文件
more +20 1.txt
// 举例4：查看的时候，增加使用者提示
more -d 1.txt
// 举例5：不滚动查看，每次翻页清除上一页
more -p 1.txt
more -c 1.txt
// 举例6：连续空行按一行显示
more -s 1.txt
// 举例7：搜索指定字符串，并从该位置向后查看
more +/serachString 1.txt
```

进入more查看文件的状态后，一些操作的快捷键：

```
Enter 向下n行，需要定义。默认为1行
Ctrl+F 向下滚动一屏
空格键 向下滚动一屏
Ctrl+B 返回上一屏
= 输出当前行的行号
V 调用vi编辑器
!命令 调用Shell，并执行命令
q 退出more
```

## Q17：mv

Linux mv（英文全拼：move file）命令用来为文件或目录改名、或将文件或目录移入其它位置。

```
// 语法
mv [options] source dest
mv [options] source... directory
```

```
// 举例1：移动某个文件到同一目录，相当于做了重命名操作
mv avahi-daemon.bak avahi-daemon
// 举例2：移动某个目录到指定目录（mv命令无需用-r移动目录）
mv temp1/ temp2/
// 举例3：移动多个文件或目录到指定目录
mv temp1/ temp2/ temp3/
```

## Q18：passwd

Linux passwd命令用来更改使用者的密码

```
passwd [-k] [-l] [-u [-f]] [-d] [-S] [username]
```

```
// 举例1：修改root用户密码
passwd root
// 举例2：修改执行用户密码
passwd testUser
```

## Q19：pwd

print working directory，打印当前目录（绝对目录）

```
// 举例1：打印当前目录
pwd
```

## Q20：rm

remove，删除文件或目录

```
// 语法
rm [options] [文件或目录]
```

```
// 举例1：删除指定文件
rm 1.txt
// 举例2：删除指定目录
rm -r /home
// 举例3：强制删除指定文件或目录（无需询问）
rm -f /home
rm -f 1.txt
// 举例4：删除当前目录下的所有文件或子目录
rm -rf *
```

## Q21：rmdir

remove empty directory，删除空目录

```
// 语法，只能删除空目录
rmdir [目录名]
```

```
// 举例1：删除指定空文件夹
rmdir /home
```

## Q22：su

Linux su（英文全拼：swith user）命令用于变更为其他使用者的身份，除 root 外，需要键入该使用者的密码。

使用权限：所有使用者。

```
// 语法
su [-fmp] [-c command] [-s shell] [--help] [--version] [-] [USER [ARG]]
```

```
// 举例1： 直接键入su，无任何option，默认切换至root用户
su
// 举例2：su root，切换至root用户
su root
// 举例3：切换至非root用户
su colin
```

## Q23：sudo

Linux sudo命令以系统管理者的身份执行指令，也就是说，经由 sudo 所执行的指令就好像是 root 亲自执行。

例如

```
// 以root用户下载
sudo apt-get install xxx
```

## Q24：tac

cat命令反过来就是tac，可以实现逆序查看文件。

```
// 举例1：逆序查看文件
tac 1.txt
// 举例2：tac没有-n选项，也就是其无法查看行号
```

## Q25：tail

tail 命令可用于查看文件的内容，有一个常用的参数 **-f** 常用于查阅正在改变的日志文件。

```
tail [参数] [文件]  
```

```
// 举例1：默认查看后10行
tail 1.txt
// 举例2：指定查看后6行
tail —n 6 1.txt
// 举例3：动态查看后10行，可用于查看日志文件
tail -f 1.txt
```

## Q26：tar

用作压缩或打包的命令，用作解压时，不同的option用于解压不同后缀的文件。

```
// 举例1：解压tar.gz格式的文件
tar —xzf test.tar.gz
// 举例2：解压tar格式的文件
tar -xf aosp-latest.tar
```

## Q27：touch

Linux touch命令用于修改文件或者目录的时间属性，包括存取时间和更改时间。若文件不存在，系统会建立一个新的文件。

ls -l 可以显示档案的时间记录。

```
// 语法
touch [-acfm][-d<日期时间>][-r<参考文件或目录>] [-t<日期时间>][--help][--version][文件或目录…]
```

```
// 举例1：文件不存在时，可用于创建文件
touch test.txt
// 举例2：touch创建的文件名不建议有空格，如需有空格，需使用双引号
touch "program files"
// 举例3：如不使用双引号，会创建多个文件（下面的例子会分别创建program和files两个文件）
touch program files
```

## Q28：useradd

Linux useradd 命令用于建立用户帐号。

useradd 可用来建立用户帐号。帐号建好之后，再用 passwd 设定帐号的密码。而可用 userdel 删除帐号。使用 useradd 指令所建立的帐号，实际上是保存在 /etc/passwd 文本文件中。

```
// 语法
useradd [-mMnr][-c <备注>][-d <登入目录>][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>][-u <uid>][用户帐号]
或
useradd -D [-b][-e <有效期限>][-f <缓冲天数>][-g <群组>][-G <群组>][-s <shell>]
```

```
// 举例1：添加用户，
// 注1：可通过查看 /etc/passwd 文件确认用户是否添加成功
// 注2：这种方法创建的用户是默认没有家目录的
useradd testUser
// 举例2：通过-m自动创建用户的家目录，一般情况要使用-m，否则会登录不上
useradd -m testUser
```

## Q29：userdel

userdel用于删除用户

```
// 语法
userdel(选项)(参数)
```

```
// 举例1：删除指定用户，此方法不会删除用户的家目录
userdel testUser
// 举例2：通过-r参数，在删除用户的同时，删除家目录
userdel -r testUser
// 举例3：通过-f参数，强制删除用户，即使当前用户已登录
userdel -f testUser
```

## Q30：w

**w[命令](https://www.linuxcool.com/)用于显示已经登陆系统的用户列表，并显示用户正在执行的指令。**

```
// 语法
w(选项)(参数)
```

```
// 举例1：显示当前活跃用户
w
```

![image-20210505102246147](.\images\image-20210505102246147.png)

## Q31：wget

wget命令用来从指定的URL下载文件。

wget与curl的区别：wget更稳定，速度更快。curl可以执行更复杂的任务。

```
// 语法
wget (选项) (参数)
```

```
// 举例1：下载某个文件，-c表示继续上次的任务，即断点续传
wget -c https://mirrors.tuna.tsinghua.edu.cn/aosp-monthly/aosp-latest.tar
```

## Q32：who

Linux who命令用于显示系统中有哪些使用者正在上面，显示的资料包含了使用者 ID、使用的终端机、从哪边连上来的、上线时间、呆滞时间、CPU 使用量、动作等等。

使用权限：所有使用者都可使用。

```
// 语法
who - [husfV] [user]
```

```
// 举例1：查看当前用户
who am i
whoami
```

# 5. Linux踩坑

## Q1：无法解析域名“cn.archive.ubuntu.com”

apt-get操作过程提示“无法解析域名“cn.archive.ubuntu.com””

![image-20210502104929391](.\images\image-20210502104929391.png)

网上说的方法，修改dns无效

当前只能吧host-only切换到桥接，可以解决。

# 6. vi/vim编辑器

## Q1：vi和vim的区别？

vim是vi的升级版本，兼容vi，但功能更强大。

## Q2：如何保存退出？如何不保存退出？

```
// 保存退出
:wq
// 不保存退出
:q!
```

## Q3：请介绍下各种插入模式？

**i-在光标位置输入**

![image-20210502113425593](.\images\image-20210502113425593.png)

**I-在光标所在行第一个非空格字符开始输入**

![image-20210502113531849](.\images\image-20210502113531849.png)

**a-在光标下一个字符后开始输入**

![image-20210502114001510](.\images\image-20210502114001510.png)

**A-在光标所在行最后一个字符后输入**

![image-20210502114024168](.\images\image-20210502114024168.png)



**o-光标所在行行后插入一行**

![image-20210502114237534](.\images\image-20210502114237534.png)

**O-在光标所在行行前插入一行**

![image-20210502114310257](.\images\image-20210502114310257.png)

## Q4：如何删除当前行

在非插入模式下，光标移动到想删除的行，按下【dd】即可

## Q5：Vim的几种模式？

正常模式：通过vim进入后的默认模式，此时按下dd可以删除整行，可以进行复制和粘贴。可通过按下I/i/O/o/A/a等进入插入模式，按下:进入命令行模式。

插入模式：正常模式下，按下I/i/O/o/A/a等进入。可进行编辑。按下Esc返回正常模式。

命令行模式：在这个模式中，可以通过相关指令，完成读取、存盘、替换、退出Vim等。按下Esc可返回正常模式。

![image-20210505103055368](.\images\image-20210505103055368.png)

# 7. Linux使用技巧

## Q1：修改重要文件时，避免可能误操作造成影响，可以如何做？

可以通过cp命令先复制一份，需要还原时，删除原问价，然后将复制的文件重命名。

例如：

```
// 创建副本
sudo cp avahi-daemon avahi-daemon.bak
// 处理原文件
sudo vi avahi-daemon
// 原文件已被改的面目全非，需使用副本文件还原
sudo rm avahi-daemon    #删除原文件
sudo mv avahi-daemon.bak avahi-daemon    #重命名
```

![image-20210502110934807](.\images\image-20210502110934807.png)

## Q2：Linux创建文件的方式有哪些？

可以通过touch命令直接创建，也可以通过vi/vim命令编辑后保存创建。

## Q3：如何更新ubuntu的软件源

```
sudo gedit /etc/apt/sources.list
```

修改该文件，根据ubuntu的版本，配置对应的软件源。

## Q4：如何在windows中查看linux的文件

可通过配置samba实现。

## Q5：如何清屏

ctrl+L或clear

# 8. Linux用户管理

## Q1：Linux用户账号管理？

Linux系统是一个多用户多任务的分时操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统

## Q2：（juju）如何增删改查用户信息？

增：useradd

删：userdel

改：usermod

查：cat /etc/passwd

## Q3：什么是组？为什么每个用户都需要属于一个组？

每个用户都有一个用户组，linux通过用户组方便对用户进行同一管理。

## Q4：（juju）查看当前用户名称有几种方法？

```
// 方法1：
id -un
// 方法2
who am i
// 方法3
whoami
```

![image-20210505101316130](.\images\image-20210505101316130.png)

```
// 其他方法
// 方法1：通过w命令查看已登录用户列表
w
```

# 9. Linux进程管理

## Q1：Linux进程创建方法？

Linux通过fork()方法创建进程:

```
#include <sys/types.h>
#include <unistd.h>
pid_t fork(void)
```

由fork()创建的新进程被称为子进程（child process）。

父、子进程完全一样（代码、数据），子进程从fork内部开始执行。

父进程中，fork返回子进程的pid；子进程中，fork返回0。

## Q2：linux进程间通信方式

管道、FIFO（命名管道）、信号

消息队列、信号量、共享内存

套接字（socket）

## Q3：进程间通信方式之：信号（signal）

信号是一种中断，用于通知接收进程某个事件已经发生。

通过如下命令可以查看linux中的信号：

```
// 查看信号列表
kill -l
```

【局限性】信号这种方式仅起到一个提示的作用，无法在进程间传递大数据。

## Q4：进程间通信方式之：管道（无名管道Pipe）

管道是单向的、先进先出的、无结构的字节流，它把一个进程的输出和另一个进程的输入连接在一起。

写进程在管道的尾端写入数据，读进程在管道的首端读出数据。

读进程试图读管道，但管道为空时，读进程阻塞；写进程试图写管道，但管道已满时，写进程阻塞。

无名管道是一种半双工的通信方式,数据只能单向流动,而且只能在具有亲缘关系的进程间使用.进程的亲缘关系一般指的是父子关系。

## Q5：linux如何通过管道实现两个进程间通信？

linux中与创建管道相关的函数：

```
// 创建一个管道，若成功则为数组fd创建两个文件描述符，其中fd[0]用于读取管道，fd[1]用于写入管道
#include <unistd.h>
int pipe(int fd[2])
```

当一个进程创建了一个管道,并调用fork创建自己的一个子进程后,父进程关闭读管道端,子进程关闭写管道端,这样提供了两个进程之间数据流动的一种方式。故可以通过父进程先调用pipe()，再调用fork()实现进程间通信。

## Q6：管道（无名管道）的局限性？

 	1. 因为读数据的同时也将数据从管道移去，因此，管道不能用来对多个接收者广播数据。
 	2. 管道中的数据被当做字节流，因此无法识别信息的边界
 	3. 如果一个管道有多个读进程，那么写进程不能发送数据到指定的读进程；同样，如果有多个写进程，那么没有办法判断是它们中哪一个发送的数据
 	4. 必须亲缘关系的进程才可通信
 	5. 管道的实体位于内核空间，所以对应的数据传输次数有两次，弱于共享内存

## Q7：进程间通信方式之：命名管道（named pipe/FIFO）

命名管道也是一种半双工的通信方式,但是它允许无亲缘关系进程间的通信。

此外，命名管道是在文件系统中作为一个特殊的文件而存在的，当共享管道的进程执行完所有的I/O操作后，命名管道将继续保存在文件系统中以便以后使用。

Linux中可通过如下函数创建命名管道：

```
// 创建一个命名管道，创建成功后会在文件系统中创建一个管道文件
#include <sys/types.h>
#include <sys/stat.h>
int mkfifo(const char *pathname, mode_t mode)
```

## Q8：进程间通信方式之：共享内存（shared memory）

共享内存区域是被多个进程共享的一部分物理内存。如果多个进程都把该内存区域映射到自己的虚拟地址空间，则这些进程都可以访问该共享内存区域，从而可以通过该区域进行通信。

共享内存是进程间共享数据的一种最快的方法，一个进程向共享内存区域写入了数据，共享这个内存区域的所有进程就可以立刻看到其中的内容。

Linux中可通过如下函数创建共享内存

```
#include <sys/shm.h>
int shmget(key_t _key, size_t _size, int _shmflg)
```

【局限性】：存在同步问题

## Q9：进程间通信方式之：信号量（semaphore）

信号量可以解决同步问题

信号量是一个整数计数器，其数值可以用于表示空闲临界资源的数量。

当有进程释放资源时，信号量增加，表示可用资源数增加；当有进程申请到资源时，信号量减少，表示可用资源数减少。

Linux中可通过如下函数创建信号量：

```
#include <sys/sem.h>
int semget(key_t _key, int _nsems, int _semflg)
```

## Q10：进程间通信方式之：消息队列（message queue）

消息队列与管道类似，但能力更强。其特点：

1. 消息队列中的消息是有类型和格式的
2. 消息队列可实现消息的随机查询。消息不一定要以先进先出次序读取。可以按消息类型读取。
3. 每个消息队列都由消息队列的标识符，消息队列的标识符在整个系统中是唯一的。
4. 消息队列允许一个或多个进程向它写入或读取消息。

Linux中可通过如下函数创建消息队列：

```
// 创建或打开消息队列
int msgget(key_t key, int msgflg);
```

## Q11：进程间通信方式之：套接字（socket）

其他6种属于本地进程间通信，socket用于网络进程间通信。