vnstat
===

监控和记录网络流量

## 安装vnStat
从特定于您的 Linux 发行版的存储库在您的系统上安装 vnStat。

例如，在 Ubuntu 上使用 apt-get 来安装它，如下所示。

```shell
apt-get install vnstat
```
```shell

wget http://humdi.net/vnstat/vnstat-1.11.tar.gz
cd vnstat-1.11
make
make install
```

请注意，您不需要像其他基于源的安装那样执行“./configure”。

由于 vnstat 依赖于内核提供的信息，因此执行以下命令来验证内核是否提供了 vnStat 所期望的所有信息。

```shell
# vnstat --testkernel
This test will take about 60 seconds.
Everything is ok.
```

2. 使用 vnStat 选择要监控的接口
vnStat 不会监控任何接口，除非您特别要求它这样做。

要开始监控 eth0，请执行以下操作。这只需要执行一次。如下所示，这将在 /var/lib/vnstat 目录下创建一个数据库文件 eth0，其中将包含此特定接口的所有网络流量日志消息。

```shell
# vnstat -u -i eth0
Error: Unable to read database "/var/lib/vnstat/eth0".
Info: -> A new database has been created.
```

要查看系统上 vnStat 可以监控的所有可用接口，请执行以下操作。

```shell
# vnstat --iflist
Available interfaces: lo eth0 eth1 sit0
```
启动 vnstatd（vnstat 守护进程），它将在后台监视和记录这些信息。

```
# vnstatd -d
# ps -ef | grep vnst
root     14353     1  0 09:12 ?        00:00:00 vnstatd -d
root     14355   330  0 09:12 pts/1    00:00:00 grep vnst
```

注意：您可以将“vnstatd -d”添加到您的 /etc/rc.local 文件中，以便在您重新启动系统时自动启动。

3. vnStat 基本用法
不带任何参数的 vnstat 将为您提供包含以下信息的快速摘要：

上次更新位于 /var/lib/vnstat/ 下的 vnStat 数据库时
从它开始收集特定接口的统计信息开始
过去两个月和过去两天的网络统计数据（传输的字节数、接收的字节数）。

```shell
# vnstat
Database updated: Sat Oct 15 11:54:00 2011

   eth0 since 10/01/11

          rx:  12.89 MiB      tx:  6.94 MiB      total:  19.82 MiB

   monthly
                     rx      |     tx      |    total    |   avg. rate
     ------------------------+-------------+-------------+---------------
       Sep '11     12.90 MiB |    6.90 MiB |   19.81 MiB |    0.14 kbit/s
       Oct '11     12.89 MiB |    6.94 MiB |   19.82 MiB |    0.15 kbit/s
     ------------------------+-------------+-------------+---------------
     estimated        29 MiB |      14 MiB |      43 MiB |

	 daily
                     rx      |     tx      |    total    |   avg. rate
     ------------------------+-------------+-------------+---------------
     yesterday      4.30 MiB |    2.42 MiB |    6.72 MiB |    0.64 kbit/s
         today      2.03 MiB |    1.07 MiB |    3.10 MiB |    0.59 kbit/s
     ------------------------+-------------+-------------+---------------
     estimated         4 MiB |       2 MiB |       6 MiB |
```

注意：如果您刚刚安装了 vnStat，它将给出以下消息“eth0：还没有足够的数据可用。”。等待一段时间，然后再次尝试该命令。

4. vnStat 小时、天、月、周网络数据
使用“vnstat -h”（或）“vnstat –hours”按小时划分网络统计数据。这也显示了一个基于文本的图表。

使用“vnstat -d”（或）“vnstat –days”按天进行网络统计数据细分。

```shell
# vnstat -d
 eth0  /  daily
         day         rx      |     tx      |    total    |   avg. rate
     ------------------------+-------------+-------------+---------------
      10/10/11      2.48 MiB |    1.28 MiB |    3.76 MiB |    0.36 kbit/s
      10/11/11      4.07 MiB |    2.17 MiB |    6.24 MiB |    0.59 kbit/s
      10/12/11      4.30 MiB |    2.42 MiB |    6.72 MiB |    0.64 kbit/s
      10/13/11      2.06 MiB |    1.10 MiB |    3.16 MiB |    0.60 kbit/s
     ------------------------+-------------+-------------+---------------
     estimated         3 MiB |       1 MiB |       4 MiB |
```

使用“vnstat -m”（或）“vnstat –months”按月划分网络统计数据。

```shell
# vnstat --m

 eth0  /  monthly

       month        rx      |     tx      |    total    |   avg. rate
    ------------------------+-------------+-------------+---------------
      Sep '11     12.90 MiB |    6.90 MiB |   19.81 MiB |    0.14 kbit/s
      Oct '11     12.92 MiB |    6.96 MiB |   19.89 MiB |    0.15 kbit/s
    ------------------------+-------------+-------------+---------------
    estimated        29 MiB |      14 MiB |      43 MiB |
```

与日和月类似，使用“vnstat -m”（或）“vnstat –months”按周进行网络统计数据细分。

5. 将数据导出到 Excel 或其他 DB
如果您希望将网络监控数据导出到excel或其他数据库，可以将数据以分号分隔的文本格式转储，您可以将其导入Excel或其他数据库。

–dumpdb 输出的第一几行包含一些标头信息。在标题行之后，它有 30 行以“d;”开头 (d;0;1318316406;1;0;386;698;1)。此行具有以分号分隔的以下信息。

d – 代表天数
0 - 当天的数字。0 表示今天。
1318316406 – Unix 格式的数据
其次，它包含发送和接收的字节

```shell
$ vnstat --dumpdb
interface;eth0
created;1218562937
updated;1218546895
totalrx;3
totaltx;1
...
...
d;0;1328316406;1;0;386;698;1
d;1;1345262937;2;1;494;289;1
```

您还可以使用“vnstat –oneline”，它在单行中显示流量摘要，其中值用分号分隔。

```shell
$ vnstat --oneline
1;eth0;10/11/11;1.45 MiB;801 KiB;2.23 MiB;0.59 kbit/s;11 年 10 月;3.93 MiB;2.06 MiB;6.00 MiB;0.05 kbit/s;3.93 MiB;2.06 MiB;6.00 MiB
```

6.显示实时网络统计
使用“vnstat -l”或“vnstat –live”显示实时网络统计信息。

```shell
$ vnstat -l
Monitoring eth0...    (press CTRL-C to stop)

   rx:        2 kbit/s     5 p/s          tx:        2 kbit/s     4 p/s
```

按 Ctrl-C 停止它后，vnstat 将显示实时监视器运行时间段的摘要。

7.更改默认的vnstat输出格式
使用“vnstat -s”或“vnstat –short”将显示网络统计信息的简短摘要。这包括今天、昨天和当月的统计数据。

```shell
$ vnstat -s (--short)

                      rx      /      tx      /     total    /   estimated
 eth0:
       Oct '11      3.93 MiB  /    2.06 MiB  /    6.00 MiB  /   13.00 MiB
     yesterday      2.48 MiB  /    1.28 MiB  /    3.76 MiB
         today      1.45 MiB  /     801 KiB  /    2.23 MiB  /      --
```

您还可以使用“vnstat –style 0”，这将给出一个窄列输出，这比默认的更宽列输出更容易阅读。

```shell
$ vnstat --style 0
以下是可用的样式编号：

0 – 窄输出
1 – 启用条形列
2 - 启用条形栏，并在摘要中显示平均流量率
3 – 在所有输出中显示平均流量
4 – 与实时模式 (vnstat -l) 结合使用时，禁用终端控制字符
8. 显示流量前 10 天
使用“vnstat -t”或“vnstat –top10”显示所有时间前 10 天的流量。
```

```
$ vnstat --top10

 eth0  /  top 10

    #      day          rx      |     tx      |    total    |   avg. rate
   -----------------------------+-------------+-------------+---------------
    1   10/12/11       4.30 MiB |    2.42 MiB |    6.72 MiB |    0.64 kbit/s
    2   10/11/11       4.07 MiB |    2.17 MiB |    6.24 MiB |    0.59 kbit/s
    3   10/10/11       2.48 MiB |    1.28 MiB |    3.76 MiB |    0.36 kbit/s
    ....
   -----------------------------+-------------+-------------+---------------
```

来源：https://bbs.huaweicloud.com/blogs/354286