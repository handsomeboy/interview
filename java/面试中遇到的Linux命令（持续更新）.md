# 面试中遇到的Linux命令（持续更新）



## 1. 目前已经遇到的命令
ps
grep
netstat

## 2. ps命令

ps：要对进程进行监测和控制,首先必须要了解当前进程的情况,也就是需要查看当前进程,而ps命令就是最基本同时也是非常强大的进程查看命令.使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵尸、哪些进程占用了过多的资源等等.总之大部分信息都是可以通过执行该命令得到的.

ps命令最常用的还是用于监控后台进程的工作情况,因为后台进程是不和屏幕键盘这些标准输入/输出设备进行通信的,所以如果需要检测其情况,便可以使用ps命令了.

注意：ps是显示瞬间进程的状态，并不动态连续；如果想对进程进行实时监控应该用**top命令**。


### 2.1. ps命令参数
>a  显示所有进程
-a 显示同一终端下的所有程序
-A 显示所有进程
c  显示进程的真实名称
-N 反向选择
-e 等于“-A”
e  显示环境变量
f  显示程序间的关系
-H 显示树状结构
r  显示当前终端的进程
T  显示当前终端的所有程序
u  指定用户的所有进程
-au 显示较详细的资讯
-aux 显示所有包含其他使用者的行程 
-C<命令> 列出指定命令的状况
--lines<行数> 每页显示的行数
--width<字符数> 每页显示的字符数
--help 显示帮助信息
--version 显示版本显示

### 2.2. ps重要命令

实例1：显示所有进程信息
命令：
ps -A
```
root@odl:/home/zln# ps -A
  PID TTY          TIME CMD
    1 ?        00:00:03 init
    2 ?        00:00:00 kthreadd
    3 ?        00:00:00 ksoftirqd/0
    5 ?        00:00:00 kworker/0:0H
    7 ?        00:00:19 rcu_sched
    8 ?        00:00:10 rcuos/0
    9 ?        00:00:09 rcuos/1

```



实例2：显示指定用户信息
命令：
ps -u root
```
root@odl:/home/zln# ps -u root
  PID TTY          TIME CMD
    1 ?        00:00:03 init
    2 ?        00:00:00 kthreadd
    3 ?        00:00:00 ksoftirqd/0
    5 ?        00:00:00 kworker/0:0H
    7 ?        00:00:19 rcu_sched
    8 ?        00:00:10 rcuos/0
    9 ?        00:00:09 rcuos/1
```

实例3：显示所有进程信息，连同命令行 
命令： 
ps -ef 
```
root@odl:/home/zln# ps -ef
root      5304     2  0 12:51 ?        00:00:00 [kworker/0:0]
root      5328   898  0 12:51 ?        00:00:00 /sbin/dhclient -d -sf /usr/lib/NetworkManager/nm-dhcp-client.action -pf /run/sendsigs.omit.d/network-manager.dhclient-eth0.pid -lf /var
zln       5421  1995  0 12:51 ?        00:00:04 gnome-terminal
zln       5430  5421  0 12:51 ?        00:00:00 gnome-pty-helper
zln       5431  5421  0 12:51 pts/0    00:00:00 bash
root      5518     2  0 13:06 ?        00:00:00 [kworker/u128:2]
root      5523  5431  0 13:07 pts/0    00:00:00 sudo su
root      5524  5523  0 13:07 pts/0    00:00:00 su
root      5525  5524  0 13:07 pts/0    00:00:00 bash
root      5554  5525  0 13:12 pts/0    00:00:00 ps -ef
```

实例4： ps 与grep 常用组合用法，查找特定进程 
命令： 
ps -ef|grep ssh 
```
root@odl:/home/zln# ps -ef|grep ssh
zln       2062  1995  0 10:57 ?        00:00:00 ssh-agent
root      5563  5525  0 13:14 pts/0    00:00:00 grep --color=auto ssh
```

实例5：将目前属于您自己这次登入的 PID 与相关信息列示出来
命令： 
ps -l 

```
root@odl:/home/zln# ps -l
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
4 S     0  5523  5431  0  80   0 - 17825 poll_s pts/0    00:00:00 sudo
4 S     0  5524  5523  0  80   0 - 17704 wait   pts/0    00:00:00 su
4 S     0  5525  5524  0  80   0 -  6320 wait   pts/0    00:00:00 bash
0 R     0  5564  5525  0  80   0 -  3553 -      pts/0    00:00:00 ps
```

实例6：列出目前所有的正在内存当中的程序 
命令： 
ps aux 
```
root@odl:/home/zln# ps auf
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
zln       5431  0.0  0.0  26944  3800 pts/0    Ss   12:51   0:00 bash
root      5523  0.0  0.0  71300  2204 pts/0    S    13:07   0:00  \_ sudo su
root      5524  0.0  0.0  70816  1812 pts/0    S    13:07   0:00      \_ su
root      5525  0.0  0.0  25280  2156 pts/0    S    13:07   0:00          \_ bash
root      5570  0.0  0.0  22640  1276 pts/0    R+   13:17   0:00              \_ ps auf
root      1188  1.8  1.6 337704 65464 tty7     Ss+  10:56   2:36 /usr/bin/X -core :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch
root      1234  0.0  0.0  20016   960 tty1     Ss+  10:56   0:00 /sbin/getty -8 38400 tty1
root      1020  0.0  0.0  20016   956 tty6     Ss+  10:56   0:00 /sbin/getty -8 38400 tty6
root      1017  0.0  0.0  20016   952 tty3     Ss+  10:56   0:00 /sbin/getty -8 38400 tty3
root      1016  0.0  0.0  20016   948 tty2     Ss+  10:56   0:00 /sbin/getty -8 38400 tty2
root      1009  0.0  0.0  20016   960 tty5     Ss+  10:56   0:00 /sbin/getty -8 38400 tty5
```

## 3. grep命令

Linux系统中grep命令是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹 配的行打印出来。grep全称是Global Regular Expression Print，表示全局正则表达式版本，它的使用权限是所有用户。

grep的工作方式是这样的，它在一个或多个文件中搜索字符串模板。如果模板包括空格，则必须被引用，模板后的所有字符串被看作文件名。搜索的结果被送到标准输出，不影响原文件内容。

grep可用于shell脚本，因为grep通过返回一个状态值来说明搜索的状态，如果模板搜索成功，则返回0，如果搜索不成功，则返回1，如果搜索的文件不存在，则返回2。我们利用这些返回值就可进行一些自动化的文本处理工作。

### 3.1. grep命令参数
>-a   --text   #不要忽略二进制的数据。   
-A<显示行数>   --after-context=<显示行数>   #除了显示符合范本样式的那一列之外，并显示该行之后的内容。   
-b   --byte-offset   #在显示符合样式的那一行之前，标示出该行第一个字符的编号。   
-B<显示行数>   --before-context=<显示行数>   #除了显示符合样式的那一行之外，并显示该行之前的内容。   
-c    --count   #计算符合样式的列数。   
-C<显示行数>    --context=<显示行数>或-<显示行数>   #除了显示符合样式的那一行之外，并显示该行之前后的内容。   
-d <动作>      --directories=<动作>   #当指定要查找的是目录而非文件时，必须使用这项参数，否则grep指令将回报信息并停止动作。   
-e<范本样式>  --regexp=<范本样式>   #指定字符串做为查找文件内容的样式。   
-E      --extended-regexp   #将样式为延伸的普通表示法来使用。   
-f<规则文件>  --file=<规则文件>   #指定规则文件，其内容含有一个或多个规则样式，让grep查找符合规则条件的文件内容，格式为每行一个规则样式。   
-F   --fixed-regexp   #将样式视为固定字符串的列表。   
-G   --basic-regexp   #将样式视为普通的表示法来使用。   
-h   --no-filename   #在显示符合样式的那一行之前，不标示该行所属的文件名称。   
-H   --with-filename   #在显示符合样式的那一行之前，表示该行所属的文件名称。   
-i    --ignore-case   #忽略字符大小写的差别。   
-l    --file-with-matches   #列出文件内容符合指定的样式的文件名称。   
-L   --files-without-match   #列出文件内容不符合指定的样式的文件名称。   
-n   --line-number   #在显示符合样式的那一行之前，标示出该行的列数编号。   
-q   --quiet或--silent   #不显示任何信息。   
-r   --recursive   #此参数的效果和指定“-d recurse”参数相同。   
-s   --no-messages   #不显示错误信息。   
-v   --revert-match   #显示不包含匹配文本的所有行。   
-V   --version   #显示版本信息。   
-w   --word-regexp   #只显示全字符合的列。   
-x    --line-regexp   #只显示全列符合的列。   
-y   #此参数的效果和指定“-i”参数相同。


### 3.2. grep重要命令

实例1：查找指定进程
命令：
ps -ef|grep svn
```
root@odl:/home/zln# ps -ef|grep svn
root      5632  5525  0 14:46 pts/0    00:00:00 grep --color=auto svn
```

实例2：查找指定进程个数
命令：
ps -ef|grep svn -c
ps -ef|grep -c svn

实例3：从文件中读取关键词进行搜索
命令：
cat test.txt | grep -f test2.txt
说明：
输出test.txt文件中含有从test2.txt文件中读取出的关键词的内容行

实例5：从文件中查找关键词
命令：
grep 'linux' test.txt

实例6：从多个文件中查找关键词
命令：
grep 'linux' test.txt test2.txt

实例7：找出已u开头的行内容
命令：
cat test.txt |grep ^u

实例8：显示包含ed或者at字符的内容行
命令：
cat test.txt |grep -E "ed|at"


## 4. netstat命令
netstat命令用于显示与IP、TCP、UDP和ICMP协议相关的统计数据，一般用于检验本机各端口的网络连接情况。netstat是在内核中访问网络及相关信息的程序，它能提供TCP连接，TCP和UDP监听，进程内存管理的相关报告。

如果你的计算机有时候接收到的数据报导致出错数据或故障，你不必感到奇怪，TCP/IP可以容许这些类型的错误，并能够自动重发数据报。但如果累计的出错情况数目占到所接收的IP数据报相当大的百分比，或者它的数目正迅速增加，那么你就应该使用netstat查一查为什么会出现这些情况了。

### 4.1. netstat命令参数

>-a或–all 显示所有连线中的Socket。
-A<网络类型>或–<网络类型> 列出该网络类型连线中的相关地址。
-c或–continuous 持续列出网络状态。
-C或–cache 显示路由器配置的快取信息。
-e或–extend 显示网络其他相关信息。
-F或–fib 显示FIB。
-g或–groups 显示多重广播功能群组组员名单。
-h或–help 在线帮助。
-i或–interfaces 显示网络界面信息表单。
-l或–listening 显示监控中的服务器的Socket。
-M或–masquerade 显示伪装的网络连线。
-n或–numeric 直接使用IP地址，而不通过域名服务器。
-N或–netlink或–symbolic 显示网络硬件外围设备的符号连接名称。
-o或–timers 显示计时器。
-p或–programs 显示正在使用Socket的程序识别码和程序名称。
-r或–route 显示Routing Table。
-s或–statistice 显示网络工作信息统计表。
-t或–tcp 显示TCP传输协议的连线状况。
-u或–udp 显示UDP传输协议的连线状况。
-v或–verbose 显示指令执行过程。
-V或–version 显示版本信息。
-w或–raw 显示RAW传输协议的连线状况。
-x或–unix 此参数的效果和指定”-A unix”参数相同。
–ip或–inet 此参数的效果和指定”-A inet”参数相同。

### 4.2. netstat重要命令

实例1：无参数使用
命令：
netstat
```
root@odl:/home/zln# netstat 
激活Internet连接 (w/o 服务器)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp6       1      0 ip6-localhost:58467     ip6-localhost:ipp       CLOSE_WAIT 
活跃的UNIX域套接字 (w/o 服务器)
Proto RefCnt Flags       Type       State         I-Node   路径
unix  22     [ ]         数据报                10310    /dev/log
unix  3      [ ]         流        已连接     17588    
unix  3      [ ]         流        已连接     13302    
unix  2      [ ]         流        已连接     19338    @/tmp/dbus-woLMbm0A
unix  3      [ ]         流        已连接     16860    
unix  3      [ ]         流        已连接     15669    @/tmp/dbus-QnB5vMC8fL
```

实例2：列出所有端口

命令：
netstat -a
```
root@odl:/home/zln# netstat -a
激活Internet连接 (服务器和已建立连接的)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 odl:domain              *:*                     LISTEN     
tcp        0      0 localhost:ipp           *:*                     LISTEN     
tcp6       0      0 ip6-localhost:ipp       [::]:*                  LISTEN     
tcp6       1      0 ip6-localhost:58467     ip6-localhost:ipp       CLOSE_WAIT 
udp        0      0 *:ipp                   *:*                                
udp        0      0 *:7042                  *:*                                
udp        0      0 *:mdns                  *:*                                
udp        0      0 *:38477                 *:*                                
udp        0      0 odl:domain              *:*                                
udp        0      0 *:bootpc                *:*                                
udp6       0      0 [::]:48124              [::]:*                             
udp6       0      0 [::]:mdns               [::]:*                             
udp6       0      0 [::]:10523              [::]:*                             
活跃的UNIX域套接字 (服务器和已建立连接的)
Proto RefCnt Flags       Type       State         I-Node   路径
unix  2      [ ACC ]     流        LISTENING     2589     /var/run/avahi-daemon/socket
unix  2      [ ACC ]     流        LISTENING     15481    @/tmp/.ICE-unix/2146
unix  2      [ ACC ]     流        LISTENING     12410    @/tmp/.X11-unix/X0

```
说明：
显示一个所有的有效连接信息列表，包括已建立的连接（ESTABLISHED），也包括监听连接请（LISTENING）的那些连接。


实例3：显示当前UDP连接状况
命令：
netstat -nu
```
root@odl:/home/zln# netstat -nu
激活Internet连接 (w/o 服务器)
Proto Recv-Q Send-Q Local Address      Foreign Address         State 
```

实例4：显示UDP端口号的使用情况
命令：
netstat -apu
```
root@odl:/home/zln# netstat -npu
激活Internet连接 (w/o 服务器)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
```

实例5：显示网卡列表
命令：
netstat -i
```
root@odl:/home/zln# netstat -i
Kernel Interface table
Iface   MTU Met   RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
eth0       1500 0      4651      0      0 0          4586      0      0      0 BMRU
lo        65536 0       771      0      0 0           771      0      0      0 LRU
```

实例6：显示网络统计信息
命令：
netstat -s
```
root@odl:/home/zln# netstat -s
Ip:
    共计收到4139个数据包
    0 已转发
    0 incoming packets discarded
    4116 incoming packets delivered
    4980请求已发出
    8 发出的报被抛弃
    18 被抛弃，因为没有路由
Icmp:
    68 ICMP messages received
    0 input ICMP message failed.
    .......
```

说明：
按照各个协议分别显示其统计数据。如果我们的应用程序（如Web浏览器）运行速度比较慢，或者不能显示Web页之类的数据，那么我们就可以用本选项来查看一下所显示的信息。我们需要仔细查看统计数据的各行，找到出错的关键字，进而确定问题所在。

实例7：显示监听的套接口
命令：
netstat -l
```
root@odl:/home/zln# netstat -l
激活Internet连接 (仅服务器)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 odl:domain              *:*                     LISTEN     
tcp        0      0 localhost:ipp           *:*                     LISTEN     
tcp6       0      0 ip6-localhost:ipp       [::]:*                  LISTEN     
udp        0      0 *:ipp                   *:*                                
udp        0      0 *:7042                  *:*                                
udp        0      0 *:mdns                  *:*                                
udp        0      0 *:38477                 *:*                                
udp        0      0 odl:domain              *:*                                
udp        0      0 *:bootpc                *:*                                
udp6       0      0 [::]:48124              [::]:*                             
udp6       0      0 [::]:mdns               [::]:*                             
udp6       0      0 [::]:10523              [::]:*                             
活跃的UNIX域套接字 (仅服务器)
Proto RefCnt Flags       Type       State         I-Node   路径
unix  2      [ ACC ]     流        LISTENING     2589     /var/run/avahi-daemon/socket
unix  2      [ ACC ]     流        LISTENING     15481    @/tmp/.ICE-unix/2146
unix  2      [ ACC ]     流        LISTENING     12410    @/tmp/.X11-unix/X0
```
实例8：显示所有已建立的有效连接
命令：
netstat -n
```
root@odl:/home/zln# netstat -n
激活Internet连接 (w/o 服务器)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp6       1      0 ::1:58467               ::1:631                 CLOSE_WAIT 
活跃的UNIX域套接字 (w/o 服务器)
Proto RefCnt Flags       Type       State         I-Node   路径
unix  22     [ ]         数据报                10310    /dev/log
unix  3      [ ]         流        已连接     17588    
unix  3      [ ]         流        已连接     13302    
unix  2      [ ]         流        已连接     19338    @/tmp/dbus-woLMbm0A
```


netstat -anp|grep 80:查看80端口是否被占用


