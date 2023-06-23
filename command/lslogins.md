lslogins
===

lslogins命令会展示出系统中现有用户的相关信息

该命令检查wtmp和btmp日志，/etc/shadow（如果需要）和 /etc/passwd并输出所需数据。

## 语法格式

```shell
lslogins [参数] 
```

## 常用参数： 

-a, --acc-expiration 显示有关上次密码更改日期和账户到期日 
--btmp-file btmp的备用路径 
-c, --colon-separate 用冒号代替换行符 
-e, --export 以NAME=VALUE格式输出数据 
-f, --failed 显示有关用户上次失败登录尝试的数据 
-G, --supp-groups 显示有关补充组的信息 
-L, --last 显示包含有关用户上次登录会话的信息的数据 
-l, --logins 仅显示登录名（用户）中指定登录名的用户的数据名称或用户名） 
-o, --output 指定要打印的输出列 
-p, --pwd 显示与按密码登录相关的信息 
-r, --raw 原始输出（无列） 
-u, --user-accs 显示用户帐户 

## 参考实例 

展示出系统中现有用户的相关信息：

```shell
lslogins -u www
```

显示与按密码登录相关的信息：

```shell
lslogins -u www -p 
```

显示有关补充组的信息：

```shell
lslogins -u www -G
```