lrzsz
===

在ssh终端中上传和下载文件

## 安装

ubuntu下

```shell
apt install lrzsz
```

或者编译安装

```shell
tar zxvf lrzsz-0.12.20.tar.gz
cd lrzsz-0.12.20
./configure
make
make install
```

> 上面安装过程默认把lsz和lrz安装到了/usr/local/bin/目录下,可以建立一个软连接

```
cd /usr/bin
ln -s /usr/local/bin/lrz rz
ln -s /usr/local/bin/lsz sz
```

上传
```
rz
```

下载
```
sz <filename>
```
