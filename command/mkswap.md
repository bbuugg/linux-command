mkswap
===

建立和设置SWAP交换分区

## 补充说明

**mkswap命令** 用于在一个文件或者设备上建立交换分区。在建立完之后要使用sawpon命令开始使用这个交换区。最后一个选择性参数指定了交换区的大小，但是这个参数是为了向后兼容设置的，没有使用的必要，一般都将整个文件或者设备作为交换区。

###  语法

```shell
mkswap(选项)(参数)
```

###  选项

```shell
-c：建立交换区前，先检查是否有损坏的区块；
-f：在SPARC电脑上建立交换区时，要加上此参数；
-v0：建立旧式交换区，此为预设值；
-v1：建立新式交换区。
```

###  参数

设备：指定交换空间对应的设备文件或者交换文件。

###  实例

 **查看系统swap space大小：** 

```shell
free -m
total used free shared buffers cached
Mem: 377 180 197 0 19 110
-/+ buffers/cache: 50 327
Swap: 572 0 572
```

 **查看当前的swap空间(file(s)/partition(s))：** 

```shell
swapon -s

等价于

cat /proc/swaps
```

 **添加交换空间** 

```shell
cat /proc/swaps # 查看交换文件
swapon -s # 查看交换文件
```

添加一个 **交换分区** 或添加一个 **交换文件** 。推荐你添加一个交换分区；不过，若你没有多少空闲空间可用，则添加交换文件。

### 添加一个交换分区，步骤如下：

使用fdisk来创建交换分区（假设 /dev/sdb2 是创建的交换分区），使用 mkswap 命令来设置交换分区：

```shell
mkswap /dev/sdb2
```

启用交换分区：

```shell
swapon /dev/sdb2
```

写入`/etc/fstab`，以便在引导时启用：

```shell
/dev/sdb2 swap swap defaults 0 0
```

### 添加一个交换文件，步骤如下：

创建大小为512M的交换文件：

```shell
dd if=/dev/zero of=/swapfile bs=1G count=1  # 创建文件，每个内存块1G，共1个内存块
```

使用mkswap命令来设置交换文件：

```shell
mkswap /swapfile
```

建议修改权限

```shell
chmod 600 /swapfile 
```

启用交换文件：

```shell
swapon /swapfile
```

写入`/etc/fstab`，以便在引导时启用：

```shell
/swapfile swap swap defaults 0 0
```

新添了交换分区并启用它之后，请查看`cat /proc/swaps`或free命令的输出来确保交换分区已被启用了。

 **删除交换空间：** 

> 在执行以上操作以后，查看你的swap分区是否满了，你首先查看一下你实际的内存剩多少空间，然后在查看自己的swap空间用了多少，首先提前保证实际剩余的内存比你的swap的内存的空间要大，然后执行一下操作，否则会宕机的

禁用交换分区：

```shell
swapon -s # 查看swap挂载位置 
swapoff /dev/sdb2 # 停止需要一段时间，需要将swap释放到物理内存中
```

禁用交换文件

```shell
swapoff /swapfile
```

从`/etc/fstab`中删除项目，使用fdisk或yast工具删除分区。


