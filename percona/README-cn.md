# 支持的tag标签，以及相关`Dockerfile`链接

-	[`5.5.46`, `5.5` (*5.5/Dockerfile*)](https://github.com/docker-library/percona/blob/038eea8c7e572f34cf01fe06aa6ce1dd29ac97e8/5.5/Dockerfile)
-	[`5.6.27`, `5.6`, `5`, `latest` (*5.6/Dockerfile*)](https://github.com/docker-library/percona/blob/038eea8c7e572f34cf01fe06aa6ce1dd29ac97e8/5.6/Dockerfile)

关于本镜像的更详细信息，请访问：[清单文件(`library/percona`)](https://github.com/docker-library/official-images/blob/master/library/percona). 镜像更新依赖于[`docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)的[`percona/tag-details.md`](https://github.com/docker-library/docs/blob/master/percona/tag-details.md) 文件。

# Percona 服务

Percona 是由Percona发起的一个 MySQL分支。

Percona 为 MySQL 数据库服务器进行了改进，在功能和性能上较 MySQL 有着很显著的提升，并保持着与MySQL的兼容。同样包含在内的还有XtraDB，由InnoDB存储影响分支出来。

> [wikipedia.org/wiki/Percona_Server](https://en.wikipedia.org/wiki/Percona_Server)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/percona/logo.png)

# 镜像的使用

## 第一个 `percona` 服务实例

简单的启动一个percona服务:

```console
$ docker run --name some-percona -e MYSQL_ROOT_PASSWORD=my-secret-pw -d percona:tag
```

... `some-percona` 是你指定的容器名称, `my-secret-pw` 指定了 MySQL root 用户的密码， `tag` 是你要运行的MySQL版本，参考前文的tag清单。

## 从其它容器中的应用连接到数据库

因为 Percona 旨在成为 MySQL 的替代品, 所以它也可以用于许多应用程序， 这个容器暴露了MySQL的端口（3360）, 所以其他应用程序容器可以使用link获得其端口。如下所示:

```console
$ docker run --name some-app --link some-percona:mysql -d application-that-uses-mysql
```

## 通过MySQL命令行客户端连接

以下命令启动了另一个 percona 容器，并通过 `mysql` 命令连接到原来 percona 容器, 实现对数据库实例的SQL语句操作:

```console
$ docker run -it --link some-percona:mysql --rm percona sh -c 'exec mysql -h"$MYSQL_PORT_3306_TCP_ADDR" -P"$MYSQL_PORT_3306_TCP_PORT" -uroot -p"$MYSQL_ENV_MYSQL_ROOT_PASSWORD"'
```

... `some-percona` 是你原来percona容器的名称.

更多的MySQL命令行客户端的相关信息可以在 [MySQL 文档](http://dev.mysql.com/doc/en/mysql.html)找到。

## 容器的Shell访问和日志查看

`docker exec` 命令允许您在Docker容器内运行命令. 下面的命令行我们将为你演示在 `percona` 容器中使用bash shell：

```console
$ docker exec -it some-percona bash
```

该Percona 的服务日志可以通过查看Docker容器的日志找到:

```console
$ docker logs some-percona
```

## 使用自定义MySQL配置文件

MySQL的启动配置文件指定为 `/etc/mysql/my.cnf`, 这个文件将包括任何在 `/etc/mysql/conf.d` 目录中的 `.cnf`文件. 该目录下的配置信息将会增加或覆盖到 `/etc/mysql/my.cnf` 配置项。 如果你想使用一个定制的MySQL配置，您可以在主机系统上创建一个配置文件的目录，并挂载到  `percona` 容器内的 `/etc/mysql/conf.d` 目录上。

例如 `/my/custom/config-file.cnf` 是一个自定义的配置文件路径, 你可以通过如下方式启动 `percona` 容器(注意,只有在如上的自定义配置文件路径中使用以下命令):


```console
$ docker run --name some-percona -v /my/custom:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=my-secret-pw -d percona:tag
```

这样启动的 `some-percona` 容器将使用`/etc/mysql/my.cnf` 和 `/etc/mysql/conf.d/config-file.cnf`两份配置, 后者决定优先级.

注意,主机系统启用了SELinux的用户可能会遇到这个问题。目前的解决方法是将相关SELinux策略类型分配给你新配置的文件,以便允许Docker安装:

```console
$ chcon -Rt svirt_sandbox_file_t /my/custom
```

## 环境变量

当你创建 `percona` 镜像时, 可以通过在 `docker run` 命令行中一个或多个环境变量来调整MySQL实例的配置. 注意 如果你启动容器的数据目录下已经包含一个数据库,下面的变量都不会有任何影响: 容器启动不会更改任何已存在的数据库。

### `MYSQL_ROOT_PASSWORD`

这个变量是强制性的,指定的密码将被设置为MySQL `root`用户的密码。在上面的例子中通过 `my-secret-pw`来实现

### `MYSQL_DATABASE`

这个变量是可选的,允许在您指定名称的数据库上创建映像启动. 如果 user/password (如下文) 也被设置，该用户在此数据库上将会有超级用户的权限 ([对应`GRANT ALL`](http://dev.mysql.com/doc/en/adding-users.html))。

### `MYSQL_USER`, `MYSQL_PASSWORD`

这些变量是可选的,用于创建一个新的用户,设置该用户的密码. 这个用户将获得`MYSQL_DATABASE`变量指定的数据库上的超级用户权限(见前文)，创建新用户需要以上两个变量都被赋值。

注意：不要以此来创建超级管理员用户, 该用户将会被默认创建并通过 `MYSQL_ROOT_PASSWORD` 变量来设置密码。

### `MYSQL_ALLOW_EMPTY_PASSWORD`

这是一个可选的变量. 设置 `yes` 以启动一个root用户密码为空的容器. *注意*: 不推荐将此变量设置为 `yes`，出发你确实需要, 因为这将使您的MySQL实例完全不设防,允许任何人获得完整的超级用户访问权限.

# 初始化一个新的实例

当容器第一次被启动时,一个新的数据库 `mysql`将通过所提供的配置参数初始化. 此外, 还将运行 `/docker-entrypoint-initdb.d`目录下的 `.sh` 和 `.sql` 文件， 您可以很容易地填充mysql服务安装SQL转储到该目录与贡献数据和提供自定义图像。 你可以[自定义镜像](https://docs.docker.com/reference/builder/)将 [SQL转储信息文件挂载到该目录下](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-file-as-a-data-volume)，然后使用提供的数据很容易的创建你自己的MySQL服务.

# 警告

## 数据存储位置

重要提示: 在Docker容器中运行的应用程序有几种方式来存储数据. 我们鼓励 `percona` 镜像用户采用的有:

-	让Docker管理你的数据库文件， [通过Docker的内部卷管理写入数据文件到主机磁盘](https://docs.docker.com/userguide/dockervolumes/#adding-a-data-volume). 这是默认的，而且简单对用户透明。 不好的地方在于，这些文件很难在主机系统上找到和使用。
-	在主机系统上创建一个数据目录并 [挂载该目录到容器内目录](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume). 这样会将数据库文件存储主机系统上的已知的位置, 使得直接在主机系统上操作数据文件变得容易。 这样做的不利在于，用户需要保证主机上的目录已存在, 并且意味着. 该目录的权限和主机系统上的其他安全机制是正确设置的。

Docker文档可以很好的帮助你理解不同的存储选项及其差异, 而且有很多博客，帖子和讨论，还提出了一些建议. 这里我们只会显示上面后一项的基本过程:

1.	在主机系统合适的卷上创建一个数据目录，如 `/my/own/datadir`.
2.	启动 `percona` 容器，如下:

	```console
	$ docker run --name some-percona -v /my/own/datadir:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d percona:tag
	```

命令行中 `-v /my/own/datadir:/var/lib/mysql` 部分是将 `/my/own/datadir` 目录挂载到容器内的 `/var/lib/mysql`处, 也就是 MySQL 默认的数据文件存储位置。

注意,主机系统启用了SELinux的用户可能会遇到这个问题. 目前的解决方案是将有关SELinux策略类型分配给新的数据目录,容器将被允许访问:

```console
$ chcon -Rt svirt_sandbox_file_t /my/own/datadir
```

## MySQL 初始化完成前，无法连接

如果没有数据库容器启动时初始化参数,那么将会创建一个默认数据库, 虽然这是预期的行为,这意味着在初始化完成之前它不会接受传入的连接. 使用自动化工具时,这可能会导致问题, 如 `docker-compose`同时启动多个容器。

## 对一个现有的数据库

如果你再一个已有数据库（尤其是 `mysql`子目录）的目录上创建容器实例 `mysql`, 此时命令行参数中应该省略`$MYSQL_ROOT_PASSWORD` ; 它将被忽略, 不会以任何方式更改到数据库。

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon

