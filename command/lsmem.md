lsmem
===

list the ranges of available memory with their online status
显示内存状态

## 用法

lsmem [options]

## 选项

Options:
  -P, --pairs          显示格式为key="value"
  -a, --all            显示每个单独的存储块
  -b, --bytes          SIZE显示为bytes
  -n, --noheadings     不显示头部标题
  -o, --output <list>  显示指定格式
  -r, --raw            显示原始格式
  -S, --split <list>   按指定列划分范围
  -s, --sysroot <dir>  指定系统根目录
      --summary[=when] 显示汇总信息,可选项为(never,always or only)

-o可选的字段列表如下
      RANGE  start and end address of the memory range
       SIZE  size of the memory range
      STATE  online status of the memory range
  REMOVABLE  memory is removable
      BLOCK  memory block number or blocks range
       NODE  numa node of memory
      ZONES  valid zones for the memory range

### 示例

```
lsmem 
```

RANGE                                  SIZE  STATE REMOVABLE BLOCK
0x0000000000000000-0x0000000007ffffff  128M online        no     0
0x0000000008000000-0x000000002fffffff  640M online       yes   1-5
0x0000000030000000-0x000000003fffffff  256M online        no   6-7
0x0000000040000000-0x0000000067ffffff  640M online       yes  8-12
0x0000000068000000-0x000000007fffffff  384M online        no 13-15

Memory block size:       128M
Total online memory:       2G
Total offline memory:      0B

```shell
$ lsmem -P
```

 -P显示格式为key="value"

RANGE="0x0000000000000000-0x0000000007ffffff" SIZE="128M" STATE="online" REMOVABLE="no" BLOCK="0"
RANGE="0x0000000008000000-0x000000002fffffff" SIZE="640M" STATE="online" REMOVABLE="yes" BLOCK="1-5"
RANGE="0x0000000030000000-0x000000003fffffff" SIZE="256M" STATE="online" REMOVABLE="no" BLOCK="6-7"
RANGE="0x0000000040000000-0x000000006fffffff" SIZE="768M" STATE="online" REMOVABLE="yes" BLOCK="8-13"
RANGE="0x0000000070000000-0x000000007fffffff" SIZE="256M" STATE="online" REMOVABLE="no" BLOCK="14-15"

```shell
$ lsmem -a
```

> -a显示所有存储块

RANGE                                  SIZE  STATE REMOVABLE BLOCK
0x0000000000000000-0x0000000007ffffff  128M online        no     0
0x0000000008000000-0x000000000fffffff  128M online       yes     1
...
0x0000000060000000-0x0000000067ffffff  128M online       yes    12
0x0000000068000000-0x000000006fffffff  128M online        no    13
0x0000000070000000-0x0000000077ffffff  128M online        no    14
0x0000000078000000-0x000000007fffffff  128M online        no    15

Memory block size:       128M
Total online memory:       2G
Total offline memory:      0B

```shell
$ lsmem -b
```

> -b显示单位为字节

RANGE                                      SIZE  STATE REMOVABLE BLOCK
0x0000000000000000-0x0000000007ffffff 134217728 online        no     0
0x0000000008000000-0x000000002fffffff 671088640 online       yes   1-5
0x0000000030000000-0x000000003fffffff 268435456 online        no   6-7
0x0000000040000000-0x0000000067ffffff 671088640 online       yes  8-12
0x0000000068000000-0x000000007fffffff 402653184 online        no 13-15

Memory block size:            134217728
Total online memory:         2147483648
Total offline memory:                 0

```shell
$ lsmem -n
```

> -n不显示标题

0x0000000000000000-0x0000000007ffffff  128M online        no     0
0x0000000008000000-0x000000002fffffff  640M online       yes   1-5
0x0000000030000000-0x000000003fffffff  256M online        no   6-7
0x0000000040000000-0x0000000067ffffff  640M online       yes  8-12
0x0000000068000000-0x000000007fffffff  384M online        no 13-15

Memory block size:       128M
Total online memory:       2G
Total offline memory:      0B

```shell
$ lsmem -r
```

> -r显示原始格式
RANGE SIZE STATE REMOVABLE BLOCK
0x0000000000000000-0x0000000007ffffff 128M online no 0
0x0000000008000000-0x000000002fffffff 640M online yes 1-5
0x0000000030000000-0x000000003fffffff 256M online no 6-7
0x0000000040000000-0x0000000067ffffff 640M online yes 8-12
0x0000000068000000-0x000000007fffffff 384M online no 13-15
# 只显示内存汇总信息
$ lsmem --summary=only
Memory block size:       128M
Total online memory:       2G
Total offline memory:      0B