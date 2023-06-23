mtr
===

My traceroute

## MTR简介

MTR（My traceroute）是几乎所有Linux操作系统发行版本预装的网络测试工具，此工具也有对应的Windows操作系统版本，名称为WinMTR。WinMTR是MTR工具在Windows操作系统下的图形化实现，但进行了功能简化，仅支持设置MTR的部分参数。

MTR工具将ping和traceroute命令的功能并入了同一个工具中，实现更强大的功能。

mtr命令默认发送ICMP数据包进行链路探测。Linux操作系统上可以通过-u参数来指定使用UDP数据包用于探测， 而Windows操作系统上，WinMTR无法切换UDP数据包。

> 参考：https://help.aliyun.com/document_detail/98706.html