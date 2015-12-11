# 简介

Joomla 是一套基于MVC框架的开源免费CMS系统。Joomla由PHP编写而成，使用面向对象(OOP)编程和设计模式，数据可存储在Mysql、MS SQL或PostgreSQL数据库中。功能特性包括页面缓存、RSS、新闻、博客、搜索以及国际化等。

> [wikipedia.org/wiki/Joomla](https://en.wikipedia.org/wiki/Joomla)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/joomla/logo.png)

# 使用

```console
$ docker run --name some-joomla --link some-mysql:mysql -d joomla
```

还可以指定如下环境变量来配置启动的实例:

-	`-e JOOMLA_DB_HOST=...` (默认是link的`mysql`容器的IP地址)
-	`-e JOOMLA_DB_USER=...` (默认为"root")
-	`-e JOOMLA_DB_PASSWORD=...` (默认是link的`mysql`容器的环境变量: `MYSQL_ROOT_PASSWORD`)
-	`-e JOOMLA_DB_NAME=...` (默认为"joomla")

如果`JOOMLA_DB_NAME`所指定的数据库在Mysql数据库中不存在，`joomla`容器将在启动的时候自动创建该数据库，请确认`JOOMLA_DB_USER`有创建数据库的权限。

如果你想从外部访问实例而不是通过容器的IP，请使用docker的端口映射参数:

```console
$ docker run --name some-joomla --link some-mysql:mysql -p 8080:80 -d joomla
```

然后，就可以使用浏览器直接访问`http://localhost:8080`或`http://host-ip:8080`

如果你想直接使用一个外部的Mysql数据库，而不是连接到一个`mysql`容器,请直接使用`JOOMLA_DB_HOST`指定数据库主机地址，`JOOMLA_DB_USER`指定账号(如果不是使用默认的root账号)，`JOOMLA_DB_PASSWORD` 指定密码:

```console
$ docker run --name some-joomla -e JOOMLA_DB_HOST=10.1.2.3:3306 \
    -e JOOMLA_DB_USER=... -e JOOMLA_DB_PASSWORD=... -d joomla
```

## 通过 [`docker-compose`](https://github.com/docker/compose) 来启动

示例 `docker-compose.yml`:

```yaml
joomla:
  image: joomla
  links:
    - joomladb:mysql
  ports:
    - 8080:80

joomladb:
  image: mysql:5.6
  environment:
    MYSQL_ROOT_PASSWORD: example
```

运行 `docker-compose up`, 等初始化完毕后，直接访问`http://localhost:8080` 或者 `http://host-ip:8080`

## 添加扩展

此镜像没有安装PHP的扩展或其他任何扩展，尽管很多流行的插件都需要，因为增加每个PHP扩展会显著增大此镜像的体积。

如果需要增加任何扩展，你需要从此镜像衍生创建`FROM`你自己的镜像。此[`php`镜像文档](https://github.com/docker-library/docs/blob/master/php/README.md#how-to-install-more-php-extensions)介绍了如何编译额外的扩展。此外，[`joomla` Dockerfile](https://github.com/joomla/docker-joomla/blob/966275ada2148e343a68c8c03870f11cc7f5b89c/apache/Dockerfile#L7-L11)也有示例。

# 授权

查看[授权](http://www.gnu.org/licenses/gpl-2.0.txt)
