# 什么是 BusyBox - 嵌入式 Linux 界的瑞士军刀

由于只有 2.5 Mb 的大小，[BusyBox](http://www.busybox.net/) 非常适合用于发行对大小非常敏感的镜像。

Busybox 包含了诸多常用的 UNIX 命令行程序的极小版本。这些命令覆盖了从 GNU fileutils 和 shellutils 等包含的实用程序。BusyBox 包含的这些程序通常比全功能的 GNU 程序支持的参数更少一些，然后被支持的参数在运行时的行为和它们的 GNU 版本基本保持了一致的行为。Busybox 为小型或者嵌入式的环境提供了一个相当完整的运行环境。


> [wikipedia.org/wiki/BusyBox](https://en.wikipedia.org/wiki/BusyBox)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/busybox/logo.png)

# 如何使用此镜像

## 运行 BusyBox shell

```console
$ docker run -it --rm busybox
```

这将使你从命令行下进行 BusyBox 的 `sh` shell 环境。

## 为二进制文件创建一个 `Dockerfile`

```dockerfile
FROM busybox
COPY ./my-static-binary /my-static-binary
CMD ["/my-static-binary"]
```

通过这个 `Dockerfile`
你可以创建一个最小化的环境来运行这个静态编译的二进制。在其它地方，比如别的容器，你将需要重新编译这个二进制。

## 更多信息

这个镜像的 tag 使用了两种不同的实现方式。`ubuntu` 基于 Ubuntu 中的`busybox-static` 包，并增加了一些额外的支持文件使其能在 Docker 下运行。因此它的构建非常快(一分钟或者更少)。
`buildroot` tag 则更复杂一些：使用 buildroot, busybox 和依赖的库和支持文件来创建整个文件系统。因此它的健壮性更强一些，同时因为使用了 uclibc 大小也会更小一些，然后需要几个小时才能完成构建。
