# 支持的镜像tag 和 `Dockerfile` 链接


-	[`12.04.5`, `12.04`, `precise-20151028`, `precise` (*precise/Dockerfile*)](https://github.com/tianon/docker-brew-ubuntu-core/blob/3c355946fd5164da3f31063a5c5f249c826f7071/precise/Dockerfile)
-	[`14.04.3`, `14.04`, `trusty-20151028`, `trusty`, `latest` (*trusty/Dockerfile*)](https://github.com/tianon/docker-brew-ubuntu-core/blob/3c355946fd5164da3f31063a5c5f249c826f7071/trusty/Dockerfile)
-	[`15.04`, `vivid-20151111`, `vivid` (*vivid/Dockerfile*)](https://github.com/tianon/docker-brew-ubuntu-core/blob/3c355946fd5164da3f31063a5c5f249c826f7071/vivid/Dockerfile)
-	[`15.10`, `wily-20151019`, `wily` (*wily/Dockerfile*)](https://github.com/tianon/docker-brew-ubuntu-core/blob/3c355946fd5164da3f31063a5c5f249c826f7071/wily/Dockerfile)

# ubuntu是什么?

Ubuntu是基于debian的linux 操作系统, 使用unity 作为默认的桌面环境. 它基于自由软件,名字来自南非ubuntu 一词, 字面意思 "人性", 常被译作"仁爱", 或者"天下共享的理念,连接起每个人" .

ubuntu的开发由南非企业家马克·沙特尔沃思创立的英国公司Canonical 领导开发.Canonical通过售卖ubuntu相关的技术支持和服务盈利.Ubuntu 项目公开承诺开源软件开发原则.人们被鼓励使用自由软件,研究它,改善它,分发它.

> [wikipedia.org/wiki/Ubuntu_(operating_system)](https://en.wikipedia.org/wiki/Ubuntu_%28operating_system%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/ubuntu/logo.png)

# 镜像里有什么?

## `/etc/apt/sources.list`

### `ubuntu:14.04`

```console
$ docker run ubuntu:14.04 grep -v '^#' /etc/apt/sources.list

deb http://archive.ubuntu.com/ubuntu/ trusty main restricted
deb-src http://archive.ubuntu.com/ubuntu/ trusty main restricted

deb http://archive.ubuntu.com/ubuntu/ trusty-updates main restricted
deb-src http://archive.ubuntu.com/ubuntu/ trusty-updates main restricted

deb http://archive.ubuntu.com/ubuntu/ trusty universe
deb-src http://archive.ubuntu.com/ubuntu/ trusty universe
deb http://archive.ubuntu.com/ubuntu/ trusty-updates universe
deb-src http://archive.ubuntu.com/ubuntu/ trusty-updates universe


deb http://archive.ubuntu.com/ubuntu/ trusty-security main restricted
deb-src http://archive.ubuntu.com/ubuntu/ trusty-security main restricted
deb http://archive.ubuntu.com/ubuntu/ trusty-security universe
deb-src http://archive.ubuntu.com/ubuntu/ trusty-security universe
```

### `ubuntu:12.04`

```console
$ docker run ubuntu:12.04 cat /etc/apt/sources.list

deb http://archive.ubuntu.com/ubuntu/ precise main restricted
deb-src http://archive.ubuntu.com/ubuntu/ precise main restricted

deb http://archive.ubuntu.com/ubuntu/ precise-updates main restricted
deb-src http://archive.ubuntu.com/ubuntu/ precise-updates main restricted

deb http://archive.ubuntu.com/ubuntu/ precise universe
deb-src http://archive.ubuntu.com/ubuntu/ precise universe
deb http://archive.ubuntu.com/ubuntu/ precise-updates universe
deb-src http://archive.ubuntu.com/ubuntu/ precise-updates universe


deb http://archive.ubuntu.com/ubuntu/ precise-security main restricted
deb-src http://archive.ubuntu.com/ubuntu/ precise-security main restricted
deb http://archive.ubuntu.com/ubuntu/ precise-security universe
deb-src http://archive.ubuntu.com/ubuntu/ precise-security universe
```

