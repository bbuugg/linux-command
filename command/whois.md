whois
===

用来查找并显示指定用户账号、域名相关信息，包括域名注册时间、拥有者、邮箱等，类似命令：tracepath、host、nslookup、who、pwd

## 介绍

whois是Linux/Unix环境下的命令，按字面意思就是问“他是谁？”，通过对域名的检索, 可以反馈回域名的注册信息，包括持有人，管理资料以及技术联络资料, 也包括该域名的域名服务器。但是在世界上有几个主要的whois服务器，它们是 whois.RIPE.net、whois.LACNIC.net、whois.APNIC.net、whois.ARIN.net，分别在各大洲。

whois的查询是逐级查询，比如当我们查询blogchina.com的时候通过亚洲的服务器whois.APNIC.net，它会返回新的whois服务器的地址：whois.melbourneit.com，而后，我们再通过 whois.melbourneit.com进行whois的查询。如果还有新的whois服务器的存在,继续往下查询，最终返回IP的详细信息。whois查询已经可以满足IP的一些基本信息的查询了，如IP的服务提供商的基本信息、IP的所属组织的基本信息、IP的网段、IP归属地等等，whois的使用手册可以从http://www.apnic.net/__data/assets/pdf_file/0004/8968/Whois-Manual-v1-Feb-2010.pdf下载，具体的whois查询所包含的内容在使用手册里面均为详细的讲解，单纯从IP本身而言，whois已经涵盖了几乎所有的基本信息。因此采用自上而下的方式去分析用户IP数据时，whois命令已经足够了。IP基本信息的数据可以完全依靠whois命令，编写相应的shell脚本处理获得。
 

## 语法

```shell
whois[选择参数][必要参数]
```

## 参数：

- -a  搜寻所有数据库
- -c  找到最小的包含一个 mnt-irt 属性的匹配
- -d  同时返回 DNS 反向查询的代理对象(需支持RPSL协议)
- -F  快速输出原始数据
- -H  隐藏法律声明内容
- -i ATTR 进行一次反向查询
- -l  将精确度降低一级的查询 (需支持RPSL协议)
- -L  找到所有低精确度匹配
- -m  找到第一级较高精确度匹配
- -M  找到所有较高精确度匹配
- -r  查询联系信息时关闭递归查询
- -R  显示本地域名对象副本
- -x  精确匹配
- -h[主机]  连接到指定 HOST 服务器
- -p[端口] 连接到指定 PORT 端口
- -t[类型]  查询指定类型对象头信息
- -T[类型]  查找指定类型的对象
- -v[类型]  查询指定类型对象冗余信息
- -q [版本|类型] 查询特定的服务器信息(需支持RPSL协议)

## 范例：

显示指定用户信息

```shell
whois root
```
 
查询域名描述信息

```shell
whois baidu.com
```

范例3：查询域名信息

```shell
whois baidu.com
```


范例4：查询域名信息省略法律声明

```shell
whois -H baidu.com
```


范例5： 指定端口查询

```shell
whois -p 80 baidu.com
```

更多whois的使用说明可以使用man whois命令进行查看。