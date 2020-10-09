# 在 Windows 上部署 Rsyslogd 的失败尝试
2020/10/09

计划部署的软件：
* `rsyslog` v8.2008.0

## 安装 Cygwin
_懂的都懂。_

安装 Cygwin 的时候，这些软件包也需要一同安装：
`gcc-core make libtool autoconf automake bison flex git`

以及 `rsyslog` 的依赖库：
`zlib-devel libgnutls-devel libuuid-devel json-c-devel libgcrypt-devel`

## 未在 Cygwin 软件源的依赖
一般只需要这两项：
* [libfastjson](https://github.com/rsyslog/libfastjson)
* [libestr](https://github.com/rsyslog/libestr)

步骤非常简单：
```sh
git clone --depth 1 https://github.com/rsyslog/libfastjson
cd libfastjson
./autogen.sh && ./configure && make && make install

git clone --depth 1 https://github.com/rsyslog/libestr
cd libestr
./autogen.sh && ./configure && make && make install
```

## 失败
获取 rsyslog 源代码：
```
git clone -b v8.2008.0 --depth 1 https://github.com/rsyslog/rsyslog
cd rsyslog
./autogen.sh --enable-static --disable-imuxsock && make && make install
```
`./autogen.sh` 能够正常运行并结束，但是 `make` 会遇到编译错误：
```
imuxsock.c:598:40: error: `SO_TIMESTAMP' undeclared (first use in this function); did you mean `PROP_TIMESTAMP'?
598|    if(setsockopt(pLstn->fd, SOL_SOCKET, SO_TIMESTAMP, &one, sizeof(one)) != 0) {
```
