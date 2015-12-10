# Supported tags and respective `Dockerfile` links

-	[`8.0`, `8` (*8.0/Dockerfile*)](https://github.com/odoo/docker/blob/6e12a6494782a2aec83346cae9c77d971b0d73b3/8.0/Dockerfile)
-	[`9.0`, `9`, `latest` (*9.0/Dockerfile*)](https://github.com/odoo/docker/blob/6e12a6494782a2aec83346cae9c77d971b0d73b3/9.0/Dockerfile)

# 什么是Odoo?

Odoo, 以前以OpenERP为人所知,是一套python编写的商业应用,使用AGPL条款发布.这套应用覆盖了所有的商业需求,从网站/电子商务到制造业,库存清单,账目清点,所有这些无缝集成一起.设法达到如此的功能覆盖,对于软件编辑者来说,这是第一次.Odoo是世界上安装最多的商业软件.世界上有2.000.000用户使用Odoo,涵盖从仅一人的小公司到300000人的大公司.

> [www.odoo.com](https://www.odoo.com)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/odoo/logo.png)

# 怎么使用镜像

镜像要求一个运行的 PostgreSQL server.

## 启动PostgreSQL容器

```console
$ docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo --name db postgres
```

## 启动Odoo 实例

```console
$ docker run -p 127.0.0.1:8069:8069 --name odoo --link db:db -t odoo
```

运行Postgres容器的别名必须是db,这样Odoo就能连接上Postgres server.

## 停止重启Odoo 实例

```console
$ docker stop odoo
$ docker start -a odoo
```

## 停止重启PostgreSQL server

当PostgreSQL被重启,链接其上的Odoo实例也需要重启,因为PostgresQL地址改变,容器链接被破坏.

重启PostgresQL不影响已经创建的数据库.

## 使用定制的配置,运行Odoo

位于`/etc/odoo/openerp-server.conf`的默认配置文件可在容器启动时用卷覆盖.设想你的定制配置放在`/path/to/config/openerp-server.conf`:

```console
$ docker run -v /path/to/config:/etc/odoo -p 127.0.0.1:8069:8069 --name odoo --link db:db -t odoo
```

请使用[这个配置模板](https://github.com/odoo/docker/blob/master/8.0/openerp-server.conf)编写你的定制配置,因为我们已经为准备运行Odoo容器,设置了部分参数. 

你也可以docker run 命令行上直接指定Odoo的参数.这些参数必须放在关键字`--`后面:

```console
$ docker run -p 127.0.0.1:8069:8069 --name odoo --link db:db -t odoo -- --db-filter=odoo_db_.*
```

## 挂载定制插件

你可以挂载自己的Odoo插件到容器内的`/mnt/extra-addons`

```console
$ docker run -v /path/to/addons:/mnt/extra-addons -p 127.0.0.1:8069:8069 --name odoo --link db:db -t odoo
```

## 运行多个Odoo实例

```console
$ docker run -p 127.0.0.1:8070:8069 --name odoo2 --link db:db -t odoo
$ docker run -p 127.0.0.1:8071:8069 --name odoo3 --link db:db -t odoo
```

请注意为了使用普通的邮件和报告功能,当主机和容器端口不同(比如 8070和8069), 其中一个必须在Odoo的设置->参数->系统参数下的 web.base.url中设置 (比如127.0.0.1:8069).

# 怎么升级本镜像

假设你使用名字是old-odoo的实例创建了一个数据库,然后要在名为new-odoo的实例中访问这个数据库,比如,由于你刚下载了一个更新的odoo镜像. 

默认,Odoo 8.0 为附件使用了文件存储(位于/var/lib/odoo/filestore/),你应该在新odoo实例中,恢复这个文件存储:

```console
$ docker run --volumes-from old-odoo -p 127.0.0.1:8070:8069 --name new-odoo --link db:db -t odoo
```

你也可通过设置在设置->参数->系统参数下的`ir_attachment.location`为`db-storage`来禁止使用文件存储.
