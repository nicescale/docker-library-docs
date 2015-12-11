# 什么是 Alpine Linux?

[Alpine Linux](http://alpinelinux.org/) 是一个基于 [musl libc](http://www.musl-libc.org/) 和 [BusyBox](http://www.busybox.net/) 的 Linux 发行版。它的镜像大小只有5MB，而且拥有比 BusyBox 更为完善的[软件包仓库](http://forum.alpinelinux.org/packages)支持。[了解更多](https://www.alpinelinux.org/about/)。

![logo](https://raw.githubusercontent.com/docker-library/docs/master/alpine/logo.png)

# 如何使用该镜像

## 使用说明

使用方式和其它的 Base 镜像类似:

```dockerfile
FROM alpine:3.1
RUN apk add --update mysql-client && rm -rf /var/cache/apk/*
ENTRYPOINT ["mysql"]
```

通过上述 Dockerfile 构建出来的镜像只有 16 MB 大小。而类似下面这个基于 Ubuntu 构建的镜像:

```dockerfile
FROM ubuntu:14.04
RUN apt-get update \
    && apt-get install -y mysql-client \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT ["mysql"]
```

这样生成的镜像大小足足有 232MB。

## 文档

[点击查看详细文档](http://gliderlabs.viewdocs.io/docker-alpine).
