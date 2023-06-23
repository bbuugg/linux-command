tcpkali
===

四层TCP协议测试利器

## 配置

### 通用配置
-h, --help : 打印出tcpkali的帮助文档并退出。
–version : 打印出tcpkali的版本号并退出
-v, --verbose level :详细打印输出结果，可以增加-v参数的次数来表示详细信息程度。例如想打印更为详细的输出结果可以使用“-vvv”。也可以使用“–verbose 3”来表示打印输出信息的详细程度。默认的详细信息程度从0-3。
-d, --dump-one : 选择在任意的一个链接上打印出该链接输入或者输出的数据。当该链接被关闭时，将会使用其他链接来进行转储信息。主要用来诊断链接中收发信息是否符合预期。
–dump-one-in : 只转储单个连接上的输入数据。
–dump-one-out : 只转储单个连接上的输出数据。
–dump-{all,all-in,all-out} : 转储所有链接上的输入输出数据。
–write-combine=off : 单独发送消息而不是批量的写入消息. 该参数设置为off则暗示着 --nagle=off, 如果没有在命令行显示调用将其关闭，这该参数默认是开启的。
-w, --workers N : 要使用的并行线程数。默认值是根据需要使用尽可能多的内核，最多使用系统中检测到的内核数量。

### 网络堆栈参数的设置
–nagle=on|off : 通过使用setsockopt()设置TCP_NODELAY套接字选项来控制Nagle算法。默认情况下根本不调用setsockopt()，这使得Nagle在大多数系统上都是启用的。
–rcvbuf SizeBytes : 设置TCP接收缓冲区(使用setsockopt()设置SO_RCVBUF套接字选项)。此选项对具有自动接收缓冲区管理的某些系统无效。如果rcvbuf不起作用，tcpkali将打印一条消息。
–sndbuf SizeBytes : 设置TCP发送缓冲区(使用setsockopt()设置SO_SNDBUF套接字选项)。此选项对具有自动接收缓冲区管理的某些系统无效。如果sndvbuf不起作用，tcpkali将打印一条消息。
–source-ip IP : 默认情况下，tcpkali自动检测当前主机端口并使用所有可用接口连接到目标主机。这个默认行为允许tcpkali打开到目的地的超过64k个连接。使用**–source- IP 通过指定要使用的特定源IP来覆盖此行为。多次指定–source-ip**选项将构建要使用的源ip列表。

### 运行测试时设置
–ws, --websocket : 使用RFC6455规范进行WebSocket通信传输。
–ssl : 为客户端和服务器端连接启用传输层安全性(TLS，以前称为SSL)。
–ssl-cert filename : 使用X.509证书来进行TLS加密传输。 默认 “cert.pem”。
–ssl-key filename :使用私钥文件来进行TLS加密传输。默认 “key.pem”。
-H, --header : 添加指定HTTP包头到WebSocket握手中。
-c, --connections N : 打开到目标的并发连接数。默认值为1。
–connect-rate Rate : 限制每秒新连接的数量。默认值是每秒100个连接。
–connect-timeout Time : 限制尝试连接中花费的时间。默认值是1秒。
–channel-lifetime Time : 在设置的时间秒数后关闭每个连接。
–channel-bandwidth-upstream Bandwidth : 限制单个连接的上行带宽。
–channel-bandwidth-downstream Bandwidth : 限制单个连接的下行带宽。
-l, --listen-port port : tcpkali作为服务端接收指定端口上的链接。
–listen-mode=silent|active : 在接收到新建连接时执行操作。在静默模式下（silent），tcpkali不发送数据并忽略接收到的数据，这是默认值。在活动模式下（active），tcpkali向连接的客户机发送消息。消息需要由message参数指定。
-T, --duration Time : 退出并在指定的时间之后打印最终统计信息。默认为10秒(-T10s)。
–delay-send Time : 将发送字节延迟到指定的时间量。

### 网络流量选项
-e, --unescape-message-args : 如在tcpkali命令行上使用了-m、-f或其他发送了流量内容的指定选项，则发送的消息数据不会进行转义。将\xxx序列转换为具有相应八进制值的字节，将\n转换为换行符，等等。
-1, --first-message : 首先发送此消息，在每个新建连接开始时发送一次。可以多次指定此选项，以便在每个连接开始时发送几个初始消息。如果–websocket选项被给出使用，每个消息都被包装到它自己的websocket框架中。
–first-message-file filename : 从文件中读取消息，并在每个连接开始时发送一次。可以多次指定此选项。
-m, --message string : 从文件中读取消息，并在每个连接开始时发送一次。可以多次指定此选项.
–message-stop string : 如果在接收到的字节流中遇到给定字符串，则终止tcpkali。
-f, --message-file filename : 重复地将从文件读取的消息发送到目标地址。可以多次指定此选项。
-r, --message-rate Rate : 在连接中每秒发送的消息数。tcpkali试图保留消息边界。此设置与–channel-bandwidth-upstream选项互不兼容，因为它们都控制消息发送速率。
-r, --message-rate @Latency : 不要指定消息速率，而是尝试计算不会导致超过给定消息延迟的最大消息速率。需要设置“ --latency-marker”延迟标记选项。

示例: tcpkali -m "PING" --latency-marker "PONG" -r @100ms

### 时延计算选项
tcpkali可以测量TCP连接延迟，tcpkali会标记指定接收特定的字节。当接收到特定标记的第一个字节时计算时间和请求-响应延迟。
–latency-connect : 测量TCP连接延迟。
–latency-first-byte : 测量第一个字节的延迟。只适用于活动套接字。
tcpkali通过重复记录每次发送消息的时间(由-m或-f指定)与在下行流量中观察到延迟标记的时间(由–latency-marker标记设置)之间的时间差来度量请求-响应延迟。
–latency-marker string : 指定要在数据流中查找的每个消息的特定字符序列。
–latency-marker-skip N : 忽略掉前N个特定字符值，该字符由–latency-marker标记。
–latency-percentiles list : 报告指定百分比的延迟。该选项采用逗号分隔的浮点值列表。平均值和最大值可以使用 --latency-percentiles 50,100报告。默认是95、99、99.5。
00
–message-marker : 被动模式下的检查或消息标记。给定此选项，tcpkali将检测{message.marker}字节序列，并将计算消息速率(以每秒消息数为单位)和消息到达延迟。在活动模式下，通过隐式启动{message.marker}}表达式来计算消息速率。

### STATSD选项
StatsD 就是一个简单的网络守护进程，基于 Node.js 平台，通过 UDP 或者 TCP 方式侦听各种统计信息，包括计数器和定时器，并发送聚合信息到后端服务

–statsd : 启用StatsD输出。默认情况下禁用StatsD输出。
–statsd-host host : 将数据发送到指定的statsd主机上。默认是本地。
–statsd-port port : 使用StatsD端口。默认是8125。
–statsd-namespace string : 指定命名空间，默认是“tcpkali”。
–statsd-latency-window Time : 默认情况下，测量延时是在tcpkali运行的整个持续时间(由–duration或-T设置)内进行。此选项指示tcpkali每次将延迟数据刷新到StatsD并重新开始测量延迟的时间。在tcpkali整个运行过程中，用户界面中显示的延迟仍然被收集。

> 参考：https://github.com/satori-com/tcpkali