# 支持的tag标签，以及相关`Dockerfile`链接

-	[`9.0.22`, `9.0` (*9.0/Dockerfile*)](https://github.com/docker-library/postgres/blob/8f8c0bbc5236e0deedd35595c504e5fd380b1233/9.0/Dockerfile)
-	[`9.1.19`, `9.1` (*9.1/Dockerfile*)](https://github.com/docker-library/postgres/blob/ed23320582f4ec5b0e5e35c99d98966dacbc6ed8/9.1/Dockerfile)
-	[`9.2.14`, `9.2` (*9.2/Dockerfile*)](https://github.com/docker-library/postgres/blob/ed23320582f4ec5b0e5e35c99d98966dacbc6ed8/9.2/Dockerfile)
-	[`9.3.10`, `9.3` (*9.3/Dockerfile*)](https://github.com/docker-library/postgres/blob/ed23320582f4ec5b0e5e35c99d98966dacbc6ed8/9.3/Dockerfile)
-	[`9.4.5`, `9.4`, `9`, `latest` (*9.4/Dockerfile*)](https://github.com/docker-library/postgres/blob/ed23320582f4ec5b0e5e35c99d98966dacbc6ed8/9.4/Dockerfile)
-	[`9.5-beta2`, `9.5` (*9.5/Dockerfile*)](https://github.com/docker-library/postgres/blob/8a9fbcb40f13ccc7762f278b9df611cabe22d300/9.5/Dockerfile)

关于本镜像的更详细信息，请访问：[清单文件 (`library/postgres`)](https://github.com/docker-library/official-images/blob/master/library/postgres). 镜像更新依赖于 [`docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于 [`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)的[`postgres/tag-details.md` 文件](https://github.com/docker-library/docs/blob/master/postgres/tag-details.md) 。

# 关于 PostgreSQL?

PostgreSQL, 常简写为 "Postgres", 是一个高可扩展性的对象-关系数据库管理系统(ORDBMS)，作为数据库，它的主要功能是安全的存储数据，并在以后可以通过其他应用程序检索到， 包括同一主机上的应用程序或是其他的网络主机上的(包括互联网). 它可以处理多种工作负载，从小型单机应用到有许多并发用户大型的面向internet应用程序， 最新版本还提供了复制数据库本身的安全性和可伸缩性。

PostgreSQL 实现了大多数的 SQL:2011 标准, 符合acid和事务(包括大多数 DDL 语言) avoiding locking issues 使用多版本并发控制 (MVCC), provides immunity to dirty reads and full serializability; 在处理复杂SQL查询时，使用了许多其他数据库无法做到的索引；可更新视图和物化视图,触发器,外键,支持函数和存储过程, 以及其它的扩展性, 并且有大量的第三方扩展，除了使用主要的自营和开源数据库, PostgreSQL还有广泛的标准SQL支持和有用的迁移工具,它的可扩展性包含了其内置的和第三方开源兼容扩展,比如Oracle的。

> [wikipedia.org/wiki/PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/postgres/logo.png)

# 镜像的使用

## 第一个 postgres 实例

```console
$ docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres
```

镜像包含 `EXPOSE 5432` (postgres 的端口), 其它容器可以通过标准的容器链接获取到链接。 创建了默认用户和数据库的 `postgres` 入口的 `initdb`.

> The postgres database is a default database meant for use by users, utilities and third party applications.  
> [postgresql.org/docs](http://www.postgresql.org/docs/9.3/interactive/app-initdb.html)

## 从应用程序连接

```console
$ docker run --name some-app --link some-postgres:postgres -d application-that-uses-postgres
```

## ... or via `psql`

```console
$ docker run -it --link some-postgres:postgres --rm postgres sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres'
```

## 环境变量

PostgreSQL的镜像使用多个环境变量，虽然没有一个变量是必需的,他们可能会大大帮助你使用镜像。

### `POSTGRES_PASSWORD`

该环境变量推荐使用，为PostgreSQL设置超级用户密码，默认的超级用户由 `POSTGRES_USER` 指定， 在上面的例子中，它被设置为"mysecretpassword".

### `POSTGRES_USER`

这个可选的环境变量与`POSTGRES_PASSWORD` 用于设置用户和密码，这个变量将创建指定的用户为超级用户，如果没有指定数据库名称，还会创建具有相同名称的数据库。如果没有指定,那么将使用默认用户的`postgres`。

### `PGDATA`

这个可选环境变量可以用来定义另一个位置，如子目录数据库文件. 默认为 `/var/lib/postgresql/data`, 但是如果你使用的卷挂载(像GCE持久磁盘), Postgres `initdb` 推荐使用一个子目录(如`/var/lib/postgresql/data/pgdata` )来存储数据。

# 镜像的扩展

如果你希望在镜像中做额外的初始化，可以 `/docker-entrypoint-initdb.d` 目录 (不存在需创建)中添加一个或多个 `*.sql` 或`*.sh` 脚本。 在 `initdb` 创建默认的 `postgres` 用户和数据库之后, 在启动服务之前将运行 `*.sql` 文件和该目录下所有 `*.sh` 脚本进一步初始化
文件会由 `POSTGRES_USER`执行, 默认 `postgres` 超级用户. 这里推荐使用 `psql` 命令和 `--username "$POSTGRES_USER"` 选项来运行 `*.sh`脚本。 由于信任验证Unix socket连接的存在使容器内这个用户将能够不用密码连接。

你同样可以使用一个简单的`Dockerfile` 来扩展镜像。如下设置了默认语言环境为 `de_DE.utf8`:

```dockerfile
FROM postgres:9.4
RUN localedef -i de_DE -c -f UTF-8 -A /usr/share/locale/locale.alias de_DE.UTF-8
ENV LANG de_DE.utf8
```

因为数据库初始化仅发生在容器启动时，这允许我们在创建之前设置语言。

# 警告

如果启动 `postgres`容器时没有数据库,  `postgres`将会创建一个默认数据库 `postgres`,这意味着它将在这段时间不接受传入的连接。这可能在使用自动化工具时导致问题,如`fig`,如同时启动多个容器。 

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon

