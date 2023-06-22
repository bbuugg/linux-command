cat
===

连接多个文件并打印到标准输出。

## 概要

连接文件并打印到标准输出设备上，`cat`经常用来显示文件的内容。`tac` 反序输出文件的内容，文件的最后一行显示在第一行。它可以对调试日志文件提供了很大的帮助，扭转日志内容的时间顺序。当文件较大时，文本在屏幕上迅速闪过（滚屏），用户往往看不清所显示的内容。因此，一般用more等命令分屏显示。为了控制滚屏，可以按Ctrl+S键，停止滚屏；按Ctrl+Q键可以恢复滚屏。按Ctrl+C（中断）键可以终止该命令的执行，并且返回Shell提示符状态。

```shell
cat [OPTION]... [FILE]...
```

## 主要用途

- 显示文件内容，如果没有文件或文件为`-`则读取标准输入。
- 将多个文件的内容进行连接并打印到标准输出。
- 显示文件内容中的不可见字符（控制字符、换行符、制表符等）。

## 参数

FILE（可选）：要处理的文件，可以为一或多个。

## 选项 

```shell
长选项与短选项等价

-A, --show-all           等价于"-vET"组合选项。
-b, --number-nonblank    只对非空行编号，从1开始编号，覆盖"-n"选项。
-e                       等价于"-vE"组合选项。
-E, --show-ends          在每行的结尾显示'$'字符。
-n, --number             对所有行编号，从1开始编号。
-s, --squeeze-blank      压缩连续的空行到一行。
-t                       等价于"-vT"组合选项。
-T, --show-tabs          使用"^I"表示TAB（制表符）。
-u                       POSIX兼容性选项，无意义。
-v, --show-nonprinting   使用"^"和"M-"符号显示控制字符，除了LFD（line feed，即换行符'\n'）和TAB（制表符）。

--help                   显示帮助信息并退出。
--version                显示版本信息并退出。
```

## 返回值

返回状态为成功除非给出了非法选项或非法参数。

## 例子 

```shell
cat ./1.log ./2.log ./3.log # 合并显示多个文件
cat -A test.log # 显示文件中的非打印字符、tab、换行符
cat -s test.log # 压缩文件的空行
cat -n test.log # 显示文件并在所有行开头附加行号
cat -b test.log # 显示文件并在所有非空行开头附加行号
echo '######' |cat - test.log # 将标准输入的内容和文件内容一并显示
cat > d.txt #从键盘创建一个文件
cat c.txt d.txt > e.txt #将几个文件合并为一个文件
cat e.txt #显示一个文件的内容
cat e.txt a.txt # 显示多个文件的内容
cat  -n e.txt # 对所有输出行编号
cat -b e.txt # 对非空输出行编号
cat -s e.txt # 如果有连续两行以上的空白行，输出时只显示一行
cat -A e.txt # 显示不可打印字符，输出时每行结尾会加上一个$
cat -n e.txt > a.txt # 将一个文件的内容加上行号后输入到另一个文件里（直接覆盖掉这个文件原来的内容）
cat -n e.txt >> a.txt # 将一个文件的内容加上行号后输入到另一个文件里（在尾部追加）
cat  e.txt > a.txt  # 复制这个文件
cat test test1 test2 test3 | sort > test4 # 合并几个文件，并且test4是已经排好序的
cat e.txt | more / less # 如果有大量的文件包含不适合在输出端子和屏幕滚动起来非常快，我们可以多和少用参数与cat命令如上表演。
```

### 注意

1. 该命令是`GNU coreutils`包中的命令，相关的帮助信息请查看`man -s 1 cat`或`info coreutils 'cat invocation'`。
2. 当使用`cat`命令查看**体积较大的文件**时，文本在屏幕上迅速闪过（滚屏），用户往往看不清所显示的内容，为了控制滚屏，可以按`Ctrl+s`键停止滚屏；按`Ctrl+q`键恢复滚屏；按`Ctrl+c`（中断）键可以终止该命令的执行，返回Shell提示符状态。
3. 建议您查看**体积较大的文件**时使用`less`、`more`命令或`emacs`、`vi`等文本编辑器。

### 参考链接

1. [Question about LFD key](https://superuser.com/questions/328054/is-there-an-lfd-key-on-my-keyboard)


