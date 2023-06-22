lsof
===

显示Linux系统当前已打开的所有文件列表 `lsof -p pid`

## 补充说明

lsof(list open files)是一个列出当前系统打开文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表对系统监测以及排错将是很有帮助的。

### 语法

```shell
lsof (选项)
```

### 选项

```shell
-a：列出打开文件存在的进程；
-c<进程名>：列出指定进程所打开的文件；
-g：列出GID号进程详情；
-d<文件号>：列出占用该文件号的进程；
+d<目录>：列出目录下被打开的文件；
+D<目录>：递归列出目录下被打开的文件；
-n<目录>：列出使用NFS的文件；
-i<条件>：列出符合条件的进程（协议、:端口、 @ip ）
-p<进程号>：列出指定进程号所打开的文件；
-u：列出UID号进程详情；
-h：显示帮助信息；
-v：显示版本信息
```

### 实例

```shell
lsof
command     PID USER   FD      type             DEVICE     SIZE       NODE NAME
init          1 root  cwd       DIR                8,2     4096          2 /
init          1 root  rtd       DIR                8,2     4096          2 /
init          1 root  txt       REG                8,2    43496    6121706 /sbin/init
init          1 root  mem       REG                8,2   143600    7823908 /lib64/ld-2.5.so
init          1 root  mem       REG                8,2  1722304    7823915 /lib64/libc-2.5.so
init          1 root  mem       REG                8,2    23360    7823919 /lib64/libdl-2.5.so
init          1 root  mem       REG                8,2    95464    7824116 /lib64/libselinux.so.1
init          1 root  mem       REG                8,2   247496    7823947 /lib64/libsepol.so.1
init          1 root   10u     FIFO               0,17                1233 /dev/initctl
migration     2 root  cwd       DIR                8,2     4096          2 /
migration     2 root  rtd       DIR                8,2     4096          2 /
migration     2 root  txt   unknown                                        /proc/2/exe
ksoftirqd     3 root  cwd       DIR                8,2     4096          2 /
ksoftirqd     3 root  rtd       DIR                8,2     4096          2 /
ksoftirqd     3 root  txt   unknown                                        /proc/3/exe
migration     4 root  cwd       DIR                8,2     4096          2 /
migration     4 root  rtd       DIR                8,2     4096          2 /
migration     4 root  txt   unknown                                        /proc/4/exe
ksoftirqd     5 root  cwd       DIR                8,2     4096          2 /
ksoftirqd     5 root  rtd       DIR                8,2     4096          2 /
ksoftirqd     5 root  txt   unknown                                        /proc/5/exe
events/0      6 root  cwd       DIR                8,2     4096          2 /
events/0      6 root  rtd       DIR                8,2     4096          2 /
events/0      6 root  txt   unknown                                        /proc/6/exe
events/1      7 root  cwd       DIR                8,2     4096          2 /
```

**lsof输出各列信息的意义如下：**

| 标识      | 说明                                                                                                              |
| :-------- | :---------------------------------------------------------------------------------------------------------------- |
| `COMMAND` | 进程的名称                                                                                                        |
| `PID`     | 进程标识符                                                                                                        |
| `PPID`    | 父进程标识符（需要指定-R参数）                                                                                    |
| `USER`    | 进程所有者                                                                                                        |
| `PGID`    | 进程所属组                                                                                                        |
| `SIZE`    | 文件的大小                                                                                                        |
| `NODE`    | 索引节点（文件在磁盘上的标识）                                                                                    |
| `NAME`    | 打开文件的确切名称                                                                                                |
| `DEVICE`  | 指定磁盘的名称                                                                                                    |
| `FD`      | 文件描述符，应用程序通过它识别该文件，应用程序通过文件描述符识别该文件。如cwd、txt等 TYPE：文件类型，如DIR、REG等 |

FD 列中的文件描述符cwd 值表示应用程序的当前工作目录，这是该应用程序启动的目录，除非它本身对这个目录进行更改,txt 类型的文件是程序代码，如应用程序二进制文件本身或共享库，如上列表中显示的 /sbin/init 程序。

其次数值表示应用程序的文件描述符，这是打开该文件时返回的一个整数。如上的最后一行文件/dev/initctl，其文件描述符为 10。u 表示该文件被打开并处于读取/写入模式，而不是只读 ® 或只写 (w) 模式。同时还有大写 的W 表示该应用程序具有对整个文件的写锁。该文件描述符用于确保每次只能打开一个应用程序实例。初始打开每个应用程序时，都具有三个文件描述符，从 0 到 2，分别表示标准输入、输出和错误流。所以大多数应用程序所打开的文件的 FD 都是从 3 开始。

与 FD 列相比，Type 列则比较直观。文件和目录分别称为 REG 和 DIR。而CHR 和 BLK，分别表示字符和块设备；或者 UNIX、FIFO 和 IPv4，分别表示 UNIX 域套接字、先进先出 (FIFO) 队列和网际协议 (IP) 套接字。

文件描述符列表：

| 标识   | 说明                                                                                                 |
| :----- | :--------------------------------------------------------------------------------------------------- |
| `cwd`  | 表示当前工作目录，即：应用程序的当前工作目录，这是该应用程序启动的目录，除非它本身对这个目录进行更改 |
| `txt`  | 该类型的文件是程序代码，如应用程序二进制文件本身或共享库，如上列表中显示的 /sbin/init 程序           |
| `lnn`  | 库引用 (AIX);                                                                                        |
| `er`   | FD 信息错误（参见名称栏）                                                                            |
| `jld`  | jail 目录 (FreeBSD);                                                                                 |
| `ltx`  | 共享库文本（代码和数据）                                                                             |
| `mxx`  | 十六进制内存映射类型编号xx                                                                           |
| `m86`  | DOS合并映射文件                                                                                      |
| `mem`  | 内存映射文件                                                                                         |
| `mmap` | 内存映射设备                                                                                         |
| `pd`   | 父目录                                                                                               |
| `rtd`  | 根目录                                                                                               |
| `tr`   | 内核跟踪文件 (OpenBSD)                                                                               |
| `v86`  | VP/ix 映射文件                                                                                       |
| `0`    | 表示标准输出                                                                                         |
| `1`    | 表示标准输入                                                                                         |
| `2`    | 表示标准错误                                                                                         |

一般在标准输出、标准错误、标准输入后还跟着文件状态模式：

| 标识   | 说明                                      |
| :----- | :---------------------------------------- |
| `u`    | 表示该文件被打开并处于读取/写入模式       |
| `r`    | 表示该文件被打开并处于只读模式            |
| `w`    | 表示该文件被打开并处于写入模式            |
| `空格` | 表示该文件的状态模式为 unknow，且没有锁定 |
| `-`    | 表示该文件的状态模式为 unknow，且被锁定   |

同时在文件状态模式后面，还跟着相关的锁：

| 标识    | 说明                                     |
| :------ | :--------------------------------------- |
| `N`     | 对于未知类型的Solaris NFS锁              |
| `r`     | 用于部分文件的读取锁定                   |
| `R`     | 对整个文件进行读取锁定                   |
| `w`     | 对文件的一部分进行写锁定(文件的部分写锁) |
| `W`     | 对整个文件进行写锁定(整个文件的写锁)     |
| `u`     | 用于任何长度的读写锁                     |
| `U`     | 对于未知类型的锁                         |
| `x`     | 对于文件部分的SCO OpenServer Xenix锁     |
| `X`     | 对于整个文件的SCO OpenServer Xenix锁     |
| `space` | 如果没有锁                               |


**文件类型**

| 标识     | 说明                           |
| :------- | :----------------------------- |
| `DIR`    | 表示目录                       |
| `CHR`    | 表示字符类型                   |
| `BLK`    | 块设备类型                     |
| `UNIX`   | UNIX 域套接字                  |
| `FIFO`   | 先进先出 (FIFO) 队列           |
| `IPv4`   | 网际协议 (IP) 套接字           |
| `DEVICE` | 指定磁盘的名称                 |
| `SIZE`   | 文件的大小                     |
| `NODE`   | 索引节点（文件在磁盘上的标识） |
| `NAME`   | 打开文件的确切名称             |
| `REG`    | 常规文件                       |

### 用法

#### 列出指定进程号所打开的文件:

```shell
lsof -p $pid
```

#### 获取端口对应的进程ID=>pid

```shell
lsof -i:9981 -P -t -sTCP:LISTEN
```

#### 列出打开文件的进程:

```shell
lsof $filename
```

#### 查看端口占用

```shell
lsof -i:$port
```

#### 列出所有打开的文件:

```shell
lsof
```

备注: 如果不加任何参数，就会打开所有被打开的文件，建议加上一下参数来具体定位

#### 查看谁正在使用某个文件

```
lsof   /filepath/file
```

#### 递归查看某个目录的文件信息

```
lsof +D /filepath/filepath2/
```

备注: 使用了+D，对应目录下的所有子目录和文件都会被列出

比使用+D选项，遍历查看某个目录的所有文件信息 的方法

```
lsof | grep ‘/filepath/filepath2/’
```

#### 列出某个用户打开的文件信息

```
lsof  -u username
```

备注: -u 选项，u其实是user的缩写

#### 列出某个程序所打开的文件信息

```
lsof -c mysql
```
备注: -c 选项将会列出所有以mysql开头的程序的文件，其实你也可以写成lsof | grep mysql,但是第一种方法明显比第二种方法要少打几个字符了

#### 列出多个程序多打开的文件信息

```
lsof -c mysql -c apache
```

#### 列出某个用户以及某个程序所打开的文件信息

```
lsof -u test -c mysql
```

#### 列出除了某个用户外的被打开的文件信息

```
lsof   -u ^root
```

备注：^这个符号在用户名之前，将会把是root用户打开的进程不让显示

#### 通过某个进程号显示该进行打开的文件

```
lsof -p 1
```

#### 列出多个进程号对应的文件信息

```
lsof -p 123,456,789
```
 
#### 列出除了某个进程号，其他进程号所打开的文件信息

```
lsof -p ^1
```

#### 列出所有的网络连接

```
lsof -i
```

#### 列出所有tcp 网络连接信息

```
lsof  -i tcp
```

#### 列出所有udp网络连接信息

```
lsof  -i udp
```

#### 列出谁在使用某个端口

```
lsof -i :3306
```

#### 列出谁在使用某个特定的udp端口

```
lsof -i udp:55
```

#### 特定的tcp端口

```
lsof -i tcp:80
```

#### 列出某个用户的所有活跃的网络端口

```
lsof  -a -u test -i
```

#### 列出所有网络文件系统

```
lsof -N
```

#### 域名socket文件

```
lsof -u
```

#### 某个用户组所打开的文件信息

```
lsof -g 5555
```

#### 根据文件描述列出对应的文件信息

```
lsof -d description(like 2)
```

#### 根据文件描述范围列出文件信息

```shell
lsof -d 2-3
```

#### 恢复删除的文件

当Linux计算机受到入侵时，常见的情况是日志文件被删除，以掩盖攻击者的踪迹。管理错误也可能导致意外删除重要的文件，比如在清理旧日志时，意外地删除了数据库的活动事务日志。有时可以通过lsof来恢复这些文件。 当进程打开了某个文件时，只要该进程保持打开该文件，即使将其删除，它依然存在于磁盘中。这意味着，进程并不知道文件已经被删除，它仍然可以向打开该文件时提供给它的文件描述符进行读取和写入。除了该进程之外，这个文件是不可见的，因为已经删除了其相应的目录索引节点。 在/proc 目录下，其中包含了反映内核和进程树的各种文件。/proc目录挂载的是在内存中所映射的一块区域，所以这些文件和目录并不存在于磁盘中，因此当我们对这些文件进行读取和写入时，实际上是在从内存中获取相关信息。大多数与 lsof 相关的信息都存储于以进程的 PID 命名的目录中，即 /proc/1234 中包含的是 PID 为 1234 的进程的信息。每个进程目录中存在着各种文件，它们可以使得应用程序简单地了解进程的内存空间、文件描述符列表、指向磁盘上的文件的符号链接和其他系统信息。lsof 程序使用该信息和其他关于内核内部状态的信息来产生其输出。所以lsof 可以显示进程的文件描述符和相关的文件名等信息。也就是我们通过访问进程的文件描述符可以找到该文件的相关信息。 当系统中的某个文件被意外地删除了，只要这个时候系统中还有进程正在访问该文件，那么我们就可以通过lsof从/proc目录下恢复该文件的内容。 假如由于误操作将/var/log/messages文件删除掉了，那么这时要将/var/log/messages文件恢复的方法如下： 首先使用lsof来查看当前是否有进程打开/var/logmessages文件，如下： 

```
lsof |grep /var/log/messages syslogd 1283 root 2w REG 3,3 5381017 1773647 /var/log/messages (deleted) 
```

从上面的信息可以看到 PID 1283（syslogd）打开文件的文件描述符为 2。同时还可以看到/var/log/messages已经标记被删除了。因此我们可以在 /proc/1283/fd/2 （fd下的每个以数字命名的文件表示进程对应的文件描述符）中查看相应的信息，如下： 

```
head -n 10 /proc/1283/fd/2 

Aug 4 13:50:15 holmes86 syslogd 1.4.1: restart. Aug 4 13:50:15 holmes86 kernel: klogd 1.4.1, log source = /proc/kmsg started. Aug 4 13:50:15 holmes86 kernel: Linux version 2.6.22.1-8 (root@everestbuilder.linux-ren.org) (gcc version 4.2.0) #1 SMP Wed Jul 18 11:18:32 EDT 2007 Aug 4 13:50:15 holmes86 kernel: BIOS-provided physical RAM map: Aug 4 13:50:15 holmes86 kernel: BIOS-e820: 0000000000000000 - 000000000009f000 (usable) Aug 4 13:50:15 holmes86 kernel: BIOS-e820: 000000000009f000 - 00000000000a0000 (reserved) Aug 4 13:50:15 holmes86 kernel: BIOS-e820: 0000000000100000 - 000000001f7d3800 (usable) Aug 4 13:50:15 holmes86 kernel: BIOS-e820: 000000001f7d3800 - 0000000020000000 (reserved) Aug 4 13:50:15 holmes86 kernel: BIOS-e820: 00000000e0000000 - 00000000f0007000 (reserved) Aug 4 13:50:15 holmes86 kernel: BIOS-e820: 00000000f0008000 - 00000000f000c000 (reserved) 
```

从上面的信息可以看出，查看 /proc/8663/fd/15 就可以得到所要恢复的数据。如果可以通过文件描述符查看相应的数据，那么就可以使用 I/O 重定向将其复制到文件中，如: cat /proc/1283/fd/2 > /var/log/messages 对于许多应用程序，尤其是日志文件和数据库，这种恢复删除文件的方法非常有用。


### 实用命令

```shell
lsof  `which httpd`                 # 那个进程在使用apache的可执行文件
lsof /etc/passwd                    # 那个进程在占用/etc/passwd
lsof /dev/hda6                      # 那个进程在占用hda6
lsof /dev/cdrom                     # 那个进程在占用光驱
lsof -c sendmail                    # 查看sendmail进程的文件使用情况
lsof -c courier -u ^zahn            # 显示出那些文件被以courier打头的进程打开，但是并不属于用户zahn
lsof -p 30297                       # 显示那些文件被pid为30297的进程打开
lsof -D /tmp                        # 显示所有在/tmp文件夹中打开的instance和文件的进程。但是symbol文件并不在列
lsof -u1000                         # 查看uid是100的用户的进程的文件使用情况
lsof -utony                         # 查看用户tony的进程的文件使用情况
lsof -u^tony                        # 查看不是用户tony的进程的文件使用情况(^是取反的意思)
lsof -i                             # 显示所有打开的端口
lsof -i:80                          # 显示所有打开80端口的进程
lsof -i -U                          # 显示所有打开的端口和UNIX domain文件
lsof -i UDP@[url]www.akadia.com:123 # 显示那些进程打开了到www.akadia.com的UDP的123(ntp)端口的链接
lsof -i tcp@ohaha.ks.edu.tw:ftp -r  # 不断查看目前ftp连接的情况(-r，lsof会永远不断的执行，直到收到中断信号,+r，lsof会一直执行，直到没有档案被显示,缺省是15s刷新)
lsof -i tcp@ohaha.ks.edu.tw:ftp -n  # lsof -n 不将IP转换为hostname，缺省是不加上-n参数
```



