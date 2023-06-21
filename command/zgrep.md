zgrep
===

使用正则表达式搜索压缩包中文件，支持 bzip2，gzip，lzip， xz 等等。

##  例子

```shell
zgrep "/api" access_log.gz
zgrep "/api" access_log.gz access_log_1.gz # 也可指定多个文件
```

##  扩展

既然提到了不解压搜索压缩包内容，.gz 的文件可以使用 zgrep ，而对于 .tar.gz 文件

```shell
zcat access.tar.gz | grep -a '/api'
zgrep -a "/api" access.tar.gz
```


其实这些带 z 的命令都包含在 Zutils 这个工具包中，这个工具包还提供了

- zcat  解压文件并将内容输出到标准输出
- zcmp  解压文件并且 byte by byte 比较两个文件
- zdiff 解压文件并且 line by line 比较两个文件
- zgrep 解压文件并且根据正则搜索文件内容
- ztest - Tests integrity of compressed files.
- zupdate - Recompresses files to lzip format.


这些命令支持 bzip2, gzip, lzip and xz 格式。

