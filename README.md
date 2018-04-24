
# 积水成河，积土成山
## Linux
>学习目标：服务器架设，Linux 系统编程，内核开发，网络开发等等
>Control + r选则以前的linux命令

### Linux重启和关闭
>登录远程虚机时，不可操作关闭系统如shutdown和halt操作，否则，关闭后，将无法再次远程登录进去，只能reboot操作；就好比，远程控制别人的远程电脑，把别人电脑关闭了，你还怎么控制；

```
- 先运行sync  //将数据由内存同步到硬盘中。不管是重启系统还是关闭系统，首先要运行 sync 命令，把内存中的数据写到磁盘中。
- Shutdown –h now 立马关机
- shutdown –h 10 //This server will shutdown after 10 mins
- reboot 就是重启，等同于 shutdown –r now
- halt 关闭系统，等同于shutdown –h now 和 poweroff
```


### Linux root权限
- 　- rw- r-- r--
表示log2012.log是一个普通文件；log2012.log的属主有读写权限；与log2012.log属主同组的用户只有读权限；其他用户也只有读权限。
- su - root 输入密码后若密码提示错误；则修改下密码就可以了；
- 修改密码为sudo passwd root来修改root权限，修改完后，可以进行对文件的编辑了 
```
可以使用 su - root登上root帐号后，使用vi /etc/hosts打开文件；
或者使用 sudo vi /etc/hosts打开文件

- vi中用：n跳转到指定的行数
- Control+r  是历史命令的调出
- mdfind 文件 //是查出文件所在的位置
- ps aux|grep tomcat //查出tomcat的进程
```

>grep （global search regular expression(RE) and print out the line,全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。
>ps命令（Process Status）就是最基本同时也是非常强大的进程查看命令

### Linux 文件和目录管理
```
- 1.mv 旧文件 新文件名
- 2.more 是基于vi编辑文本过滤器（空格下一屏，enter键是下一行，b是上一屏，Q是退出，斜线输入查找，h键进行帮助）；
   -   more -dc file //显示文件file的内容，但在显示之前先清屏，并且在屏幕的最下方显示完核的百分比
   - more -c -10 file //显示文件file的内容，每10行显示一次，而且在显示之前先清屏
   - d：显示“[press space to continue,'q' to quit.]”和“[Press 'h' for  instructions]”；
   - c：不进行滚屏操作。每次刷新这个屏幕；
   - s：将多个空行压缩成一行显示；
   - u：禁止下划线；
- 3. head命令是显示文件的开头的内容，默认显示前10行  
- 4. tail 命令 tail file （显示文件file的最后10行）
    - tail +20 file （显示文件file的内容，从第20行至文件末尾）
    - tail -c 10 file （显示文件file的最后10个字符）
- 5. less 的命令的作用和more相似，显示文件用PageUp和PageDown来翻页
    - e：文件内容显示完毕后，自动退出；
    - f：强制显示文件；
    - g：不加亮显示搜索到的所有关键词，仅显示当前显示的关键字，以提高显示速度；
    - l：搜索时忽略大小写的差异；
    - N：每一行行首显示行号；
    - s：将连续多个空行压缩成一行显示；
    - S：在单行显示较长的内容，而不换行显示；
    - x<数字>：将TAB字符显示为指定个数的空格字符。
- 6. find . -name "*.sh" //搜索在当前目录下及子目录下所有的以.sh文件结尾的结果
    - find /usr/ -path "*local*" //匹配文件路径或者文件
    - find . -name "*.txt" -o -name "*.pdf"  //当前目录及子目录下查找所有以.txt和.pdf结尾的文件
- 7. whereis //whereis命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。
- 8. which 命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。
- 9. diff /usr/li test.txt //用于比较两个文件的不同之处；
- 10 diff3 //比较三个文件的不同
- 11 strings //用于打印文件的字符，可以搜索 eg. strings /bin/test.sh |grep -i guan //查询/bin/test.sh中guan的字符，-i意思为不区分大小写（ignore）；
- 12 cmp ssh200.sh ssh202.sh //比较两个文件的差别，并列出差别的地方如 differ: char 16, line 2  //汉字字符是两个字节，英文字符是一个字节，字符>=字节
- 13 tar -xvf file.tar //解压tar包，（学习tar包，war包，rpm包的区别）
- 14 配置 ll命令 $ vim ~/.bashrc
                -alias ll='ls -l'   #加入此行
                 -ps:加入后肯能无法当场起作用,
                 -执行该句: source ~/.bashrc
- 15 软链接 ln -s httpd.conf confighttp  //    confighttp是链接文件名， httpd.conf是源文件，用rm -rf 删除confighttp只是删除链接文件，而不会删除源文件
- 16 硬链接 ln httpd.conf confighttp   //硬链接文件相当于一个文件存储在两个位置，可以有效搜索防止误删。
- 17 linux中ll文件时显示白色的目录，需要设置  cp /etc/DIR_COLORS /root/ //如果是root用户，就复制到root下并改名为.dir_colors，修改“DIR 01;34 01表示是黑体，34是蓝色，

并编辑~/.bashrc）修改为
     export LS_OPTIONS='--color=auto'
     eval "`dircolors`"
     alias ls='ls $LS_OPTIONS'
     alias ll='ls $LS_OPTIONS -l'
     alias l='ls $LS_OPTIONS -lA'   
- 18 echo "*/5 * * * * root temp.sh">>/etc/crontab //每5分钟执行一次脚本 
- 19 rmdir 删除空目录  （rmdir -p 是当子目录被删除后，使他也成为空目录的话，则顺便一并删除）
- 20 tree tree -C 颜色显示, tree -f 显示文件全路径 ,tree -L 2只显示2层,tree -P *.pl只显示文件目录和*.pl的perl文件。tree -F显示目录后面的\；显示可执行文件*；功能类似ls -F,tree –help帮助手册。ps：linux所有命令，都可以用--help去扩展思路。总结tree -FC应该是最最常用的。
-21 wc命令 （word count）缩写，统计指定文件中的字节数、字数、行数，并将统计结果显示输出。
    - wc test.txt //统计文件的字节数、字数、行数
    - wc -l test.txt //统计行数 或者 cat test.txt| wc -l 只显示行数
    - ls -l | wc -l //统计文件数，包括当前文件   
```           

### Linux下备份数据库
```
博客地址
https://www.cnblogs.com/CHEUNGKAMING/p/5717455.html
cron是一个linux下 的定时执行工具，可以在无需人工干预的情况下运行作业。
　　service crond start    //启动服务
　　service crond stop     //关闭服务
　　service crond restart  //重启服务
　　service crond reload   //重新载入配置
　　service crond status   //查看服务状态 

- 1. #!/bin/sh mysqldump -uuser -ppassword dbname | gzip > /var/lib/mysqlbackup/dbname`date +%Y-%m-%d_%H%M%S`.sql.gz cd  /var/lib/mysqlbackup rm -rf `find . -name '*.sql.gz' -mtime 10`/usr/yunji/backup_mysql //脚本
- 2. chmod +x dbbackup.sh //更改备份脚本权限
- 3. crontab -e （etc/crontab是系统备份，而crontab -e属于某个属猪的备份） 若每天晚上21点00备份，添加如下代码
00 21 * * * /var/lib/mysqlbackup/dbbackup.sh

```


### Linux网络管理命令
```
- 1.ifconfig查询ip命令
- 2.netstat -ntlp //查询当前端口列表
    - 1.netstat -nu   //显示当前户籍UDP连接状况
    - 2.netstat -apu  //显示udp端口号的使用情况
    - 3.netstat -i    //显示网卡列表
    - 4.netstat -an   //查看所有正在使用的端口
    - 5.netstat -tln 8080 //查看什么进程占用8080端口，返回pid，查看端口号可以看一个tomcat中的一个服务是否起来
- 3.lsof -i:8080      //显示网络统计信息
- 4.ps -ef|grep tomcat //或者端口号，查看服务信息或者中间件的信息
- 5.ps axu|grep pid    //查看进程的具体信息   
- 6.traceroute 跟踪数据包到达网络主机所经过的路由工具；
    traceroute 是用来发出数据包的主机到目标主机之间所经过的网关的工具
- 7. ss 命令 
     - （ss是Socket Statistics的缩写）ss命令可以用来获取socket统计信息，它可以显示和netstat类似的内容。但ss的优势在于它能够显示更多更详细的有关TCP和连接状态的信息，而且比netstat更快速更高效。
     - ss -t -a 显示tcp连接 
     - ss -s    //显示sockets摘要
     - ss -l    //列出所有打开的网络接口
     - ss -pl   //查看进程使用的socket
     - ss -lp | grep 3306 //找出打开套接字/端口应用程序
     - ss -u -a //显示所有UDP Sockets
     - ss -o state established '( dport = :smtp or sport = :smtp )' //显示所有状态为established的SMTP连接
     - ss -o state established '( dport = :http or sport = :http )' //显示所有状态为Established的HTTP连接
     - ss -o state fin-wait-1 '( sport = :http or sport = :https )' dst 193.233.7/24 //列举出处于 FIN-WAIT-1状态的源端口为 80或者 443，目标网络为 193.233.7/24所有 tcp套接字命令
     - ss src ADDRESS_PATTERN 匹配本地地址和端口号

          - ss src 192.168.119.103

          - ss src 192.168.119.103:http

          - ss src 192.168.119.103:80

          - ss src 192.168.119.103:smtp

          - ss src 192.168.119.103:25
-8 time命令 //校验ss和netstat的使用效率 time netstat -at 和time ss
```

### Linux系统管理
```
- getconf LONG_BIT //判断Linux是32位还是64位的
- uname -r //查看kernel版本，判断Linux是32位还是64位的
- uname -a //查看系统版本
- 在tomcat下的bin目录下，可以使用sh version.sh 查看tomcat及jvm的信息

```
### Linux软件.打印.开发.工具

```
- clockdiff -o 加ip //比较两个系统主机的时间差
- dircolors //用于设置 ls 指令在显示目录或文件时所用的色彩
- debian 或者 ubuntu : sudo apt-get install wget//安转wget
- centos : sudo yum -y install wget
```

### Linux硬件.内核.Shell.监测



### 设置时区(中国的时区)
>换句话说就是tzselect命令仅仅告诉我们通过设置TZ这个环境变量来选择的时区，然后将变量添加到.profile文件中。

```
tzselect ->选择Asia->选择China->选择北京——>yes退出即可，

```

### Linux下配备nginx
```
- 启动 /usr/local/nginx/sbin/nginx
- 检查是否启动 netstat -tlunp 或 ps aux | grep nginx
- nginx -s stop  快速关闭nginx
- nginx -s quit  平滑关闭nginx
- kill -s QUIT 11247  通过linux的kill命令杀死nginx进程，11247为nginx的主进程号
- nginx -s reload  修改了nginx的配置文件后，可以使用该命令让新的配置立即生效，而不用重启整个nginx服务器

```
### Linux下的mysql的安装
依次执行命令：
sudo rpm -ivh --force mysql-community-common-5.7.16-1.el6.x86_64.rpm 
sudo rpm -ivh --force mysql-community-libs-5.7.16-1.el6.x86_64.rpm
sudo rpm -ivh --force mysql-community-client-5.7.16-1.el6.x86_64.rpm
sudo rpm -ivh --force mysql-community-server-5.7.16-1.el6.x86_64.rpm

cat /var/log/mysqld.log | grep password  //在文件中搜索关键字

### 校正时间
```
date命令将日期设置为2014年6月18日

 ----   date -s 06/18/14

将时间设置为14点20分50秒

 ----   date -s 14:20:50

将时间设置为2014年6月18日14点16分30秒（MMDDhhmmYYYY.ss）

----date 0618141614.30

```


#### vi命令
```
- x  是删除的意思
- shift+g 是切换到最后一行
- gg  跳至文件的第一行
- dd 是删除一行数据
- a 是插入当前位置后面
- i 是插入到当前位置前面
- oo 是新增一行到当前位置的下面
- OO 是新增一行到当前位置的上面
- :n 跳转到指定的行数
- :w 将缓冲区写入文件，即保存修改
- :wq 保存修改并退出
- :x 保存修改并退出
- :q 退出，如果对缓冲区进行过修改，则会提示
- :q! 强制退出，放弃修改
- n 下一个匹配(如果是/搜索，则是向下的下一个，?搜索则是向上的下一个)
- N  上一个匹配(同上)
- ^ 跳至行首的第一个字符
- $ 跳至行尾
- u 是返回上一步
```
#### shell
```
#!/bin/bash。
#! 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。
echo //用于向窗口输出文本
chmod +x 脚本文件 //加上脚本的执行权限
./test.sh 或者 sh test.sh //都可以执行此脚本（注：必须用./执行脚本时必须加‘./’,Linux中默认是从path中找文件，加上./是默认从当前目录下找文件）

```

### rpm包管理
>RMP 是 LINUX 下的一种软件的可执行程序，你只要安装它就可以了。这种软件安装包通常是一个RPM包（Redhat Linux Packet Manager，就是Redhat的包管理器），后缀是.rpm。 
RPM是Red Hat公司随Redhat Linux推出了一个软件包管理器，通过它能够更加轻松容易地实现软件的安装。 
```
1.安装软件：执行rpm -ivh rpm包名，如： 
 rpm -ivh apache-1.3.6.i386.rpm 
2.升级软件：执行rpm -Uvh rpm包名。 
3.反安装：执行rpm -e rpm包名。 
4.查询软件包的详细信息：执行rpm -qpi rpm包名 
5.查询某个文件是属于那个rpm包的：执行rpm -qf rpm包名 
6.查该软件包会向系统里面写入哪些文件：执行 rpm -qpl rpm包名
```
### 广发添加dns解析

```
[root@idcos-10-2-68-197 ~]# more /etc/resolv.conf
# Generated by NetworkManager
search gf.com.cn
nameserver 10.35.88.77
nameserver 10.2.66.66
```



### 网络学习
```
网关：网关(Gateway)又称网间连接器、协议转换器。网关在网络层以上实现网络互连，是最复杂的网络互连设备，仅用于两个高层协议不同的网络互连。


```



大树英格澜（嘉兴高铁南站）
润江花园、大运府邸、大众湖滨花园（嘉善）
嘉兴金地都会艺境
御景苑海宁
景润苑

