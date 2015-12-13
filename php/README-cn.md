# 支持的tag和其 `Dockerfile` 链接地址

-	[`5.4.45-cli`, `5.4-cli`, `5.4.45`, `5.4` (*5.4/Dockerfile*)](https://github.com/docker-library/php/blob/fec7f537f049aafd2102202519c3ca9cb9576707/5.4/Dockerfile)
-	[`5.4.45-apache`, `5.4-apache` (*5.4/apache/Dockerfile*)](https://github.com/docker-library/php/blob/fec7f537f049aafd2102202519c3ca9cb9576707/5.4/apache/Dockerfile)
-	[`5.4.45-fpm`, `5.4-fpm` (*5.4/fpm/Dockerfile*)](https://github.com/docker-library/php/blob/fec7f537f049aafd2102202519c3ca9cb9576707/5.4/fpm/Dockerfile)
-	[`5.5.30-cli`, `5.5-cli`, `5.5.30`, `5.5` (*5.5/Dockerfile*)](https://github.com/docker-library/php/blob/fec7f537f049aafd2102202519c3ca9cb9576707/5.5/Dockerfile)
-	[`5.5.30-apache`, `5.5-apache` (*5.5/apache/Dockerfile*)](https://github.com/docker-library/php/blob/fec7f537f049aafd2102202519c3ca9cb9576707/5.5/apache/Dockerfile)
-	[`5.5.30-fpm`, `5.5-fpm` (*5.5/fpm/Dockerfile*)](https://github.com/docker-library/php/blob/fec7f537f049aafd2102202519c3ca9cb9576707/5.5/fpm/Dockerfile)
-	[`5.6.15-cli`, `5.6-cli`, `5-cli`, `cli`, `5.6.15`, `5.6`, `5`, `latest` (*5.6/Dockerfile*)](https://github.com/docker-library/php/blob/6e600f59a4405b5066eb78f5f02180212dffd065/5.6/Dockerfile)
-	[`5.6.15-apache`, `5.6-apache`, `5-apache`, `apache` (*5.6/apache/Dockerfile*)](https://github.com/docker-library/php/blob/6e600f59a4405b5066eb78f5f02180212dffd065/5.6/apache/Dockerfile)
-	[`5.6.15-fpm`, `5.6-fpm`, `5-fpm`, `fpm` (*5.6/fpm/Dockerfile*)](https://github.com/docker-library/php/blob/6e600f59a4405b5066eb78f5f02180212dffd065/5.6/fpm/Dockerfile)
-	[`7.0.0RC7-cli`, `7.0-cli`, `7-cli`, `7.0.0RC7`, `7.0`, `7` (*7.0/Dockerfile*)](https://github.com/docker-library/php/blob/ab08029415ca6e0c7a9047d6016946507a20fcb0/7.0/Dockerfile)
-	[`7.0.0RC7-apache`, `7.0-apache`, `7-apache` (*7.0/apache/Dockerfile*)](https://github.com/docker-library/php/blob/ab08029415ca6e0c7a9047d6016946507a20fcb0/7.0/apache/Dockerfile)
-	[`7.0.0RC7-fpm`, `7.0-fpm`, `7-fpm` (*7.0/fpm/Dockerfile*)](https://github.com/docker-library/php/blob/ab08029415ca6e0c7a9047d6016946507a20fcb0/7.0/fpm/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/php`)](https://github.com/docker-library/official-images/blob/master/library/php). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# What is PHP?
PHP是一个为Web开发设计的服务器端脚本语言，但它也可以作为一个通用的编程语言，PHP可以直接添加到HTML或它可以用于各种模版引擎和Web框架，PHP代码通常由解释器处理， 在web-server上实现一个本地模块或者作为一个公共网关接口(CGI)

> [wikipedia.org/wiki/PHP](http://en.wikipedia.org/wiki/PHP)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/php/logo.png)

# 如何使用此镜像.

## 用命令行

对于PHP项目通过命令行界面（CLI）运行，你可以跟着下面做.

### 在你的php项目下创建一个 `Dockerfile` 文件

```dockerfile
FROM php:5.6-cli
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
CMD [ "php", "./your-script.php" ]
```

然后, 运行命令去build和run这个docker镜像:

```console
$ docker build -t my-php-app .
$ docker run -it --rm --name my-running-app my-php-app
```

### 运行一个单独的PHP脚本

对于需要简单的,单独的文件项目,你也许不方便去写一个完整的`Dockerfile`文件,在这种情况下,你可以立即用PHP Docker镜像运行一个PHP脚本

```console
$ docker run -it --rm --name my-running-script -v "$PWD":/usr/src/myapp -w /usr/src/myapp php:5.6-cli php your-script.php
```

## 与Apache

通常, 你可能想在Apache httpd下运行PHP, 方便的是，有一个版本的PHP容器包含Apache web服务

### 在你的PHP项目下创建一个 `Dockerfile` 文件

```dockerfile
FROM php:5.6-apache
COPY src/ /var/www/html/
```
在 `src/` 文件夹里包含你的所有php代码. 然后 运行命令去构建和运行Docker镜像

```console
$ docker build -t my-php-app .
$ docker run -it --rm --name my-running-app my-php-app
```

我们建议你添加一个自定义的`php.ini`配置. 经过在Dockerfile上加一行可以`COPY`到`/usr/local/etc/php`目录下,并且运行相同的命令去构建和运行

```dockerfile
FROM php:5.6-apache
COPY config/php.ini /usr/local/etc/php/
COPY src/ /var/www/html/
```

在 `src/` 文件夹里包含你的所有php代码并且`config/`里面有你的`php.ini`文件

### 怎样安装更多的PHP扩展

我们提供两个实用脚本,`docker-php-ext-configure` 和 `docker-php-ext-install`, 你可以用他们更容易的安装PHP扩展

例如, 如果你想有一个带有`iconv`, `mcrypt` and `gd`扩展的PHP-FPM镜像, 如果你喜欢你可以继承自基础镜像,并且像这样去写你自己的`Dockerfile`

```dockerfile
FROM php:5.6-fpm
# Install modules
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
    && docker-php-ext-install iconv mcrypt \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd
CMD ["php-fpm"]
```

记住，你必须手动安装你的扩展依赖, 如果扩展需要自定义`configure`参数, 你可以像这个例子一样使用`docker-php-ext-configure`脚本

### 不用`Dockerfile`

如果你不想在你的项目里面放`Dockerfile`, 下面这样就足够了

```console
$ docker run -it --rm --name my-apache-php-app -v "$PWD":/var/www/html php:5.6-apache
```
# 许可证

查看此镜像所包含软件的[许可证信息](http://php.net/license/).

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.

# 用户反馈

## 文档

该镜像文档放在 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[`perl/` directory](https://github.com/docker-library/docs/tree/master/php) .
提交pull request之前 确定你已经熟悉了[repository's `README.md` file](https://github.com/docker-library/docs/blob/master/README.md)

## 问题

关于此镜像你有任何的问题或者疑问, 请通过 [GitHub issue](https://github.com/Perl/docker-perl/issues)联系我们.

你也可以通过在[Freenode](https://freenode.net)上的`#docker-library` IRC频道取得和官方镜像维护者的联系

## 贡献代码

邀请你贡献无论大小的新功能, 修复bug, 或者更新; 我们一直很开心收到pull request, 并且我们会快速的处理好他们

在开始写代码前, 我们建议你在[GitHub issue](https://github.com/docker-library/php/issues)讨论你的方案, 尤其是更重大的代码贡献. 这样其他的贡献者就能有机会引导你去正确的方向上,给你的设计提出反馈,并且帮助你找到是否有其他人在做同样的事情.
