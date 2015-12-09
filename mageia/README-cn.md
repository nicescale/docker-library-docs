# 简介

[Mageia](http://www.mageia.org) 是一个基于GNU/linux的免费操作系统。由[megeia社区](https://www.mageia.org/en/community/)主导。

![logo](https://raw.githubusercontent.com/docker-library/docs/master/mageia/logo.png)

# 使用

## 创建Dockerfile

```dockerfile
FROM mageia:4
MAINTAINER  "Foo Bar" <foo@bar.com>
CMD [ "bash" ]
```

## 已安装的包

镜像中已安装了如下包:

-	basesystem-minimal
-	urpmi
-	locales
-	locales-en
