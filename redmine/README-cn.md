# Redmine 2.6.8 , 3.0.6  & Passenger 5.0.21  Dockerfile 

### Redmine 是什么?

Redmine 是一个自由开源基于web 的项目管理和问题追踪工具. 允许用户管理多个项目及其关联子项目.它提供每项目维基,论坛,时间追踪,灵活的基于角色的访问控制. 它包括日历和gantt 图表,用以辅助可视化表达项目及其截止时间.Redmine 集成了多种版本控制工具,包括仓库浏览器, 差异观察器.

> [wikipedia.org/wiki/Redmine](https://en.wikipedia.org/wiki/Redmine)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/redmine/logo.png)

### 镜像使用

#### 结合sqlite3使用

```console
$ docker run -d --name some-redmine redmine
```

> 不适合多用户的产品使用 ([redmine wiki](http://www.redmine.org/projects/redmine/wiki/RedmineInstall#Supported-database-back-ends))

#### 结合数据库容器使用(推荐)

1.	启动数据库容器

	-	PostgreSQL

		```console
		$ docker run -d --name some-postgres -e POSTGRES_PASSWORD=secret -e POSTGRES_USER=redmine postgres
		```

	-	MySQL (使用 `--link some-mysql:mysql` 替换  `--link some-postgres:postgres`)

		```console
		$ docker run -d --name some-mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=redmine mysql
		```

2.	启动redmine容器

	```console
	$ docker run -d --name some-redmine --link some-postgres:postgres redmine
	```

### webserver 的其他选择

默认tag redmine 使用了WEBrick 做web server,  仓库里的passenger 子目录提供了使用passenger 做web server,与默认版本使用同样的环境变量及容器链接配置.`passenger` 使用了 [Phusion Passenger](https://www.phusionpassenger.com/). [`tini`](https://github.com/krallin/tini) 被用于回收 [僵尸进程](https://en.wikipedia.org/wiki/Zombie_process).

### 访问应用

目前, 默认用户名,密码是 admin/admin ([logging into the application](https://www.redmine.org/projects/redmine/wiki/RedmineInstall#Step-10-Logging-into-the-application)).

### 数据的存储

-	让docker来管理你的数据文件 [使用数据卷](https://docs.docker.com/userguide/dockervolumes/#adding-a-data-volume). 这是默认行为而且对于用户是透明的. 这种方式的缺点是主机系统上的应用,工具不太容易定位数据位置
-	在主机上创建一个数据目录(容器外部) 然后 [将其挂载进入容器内部](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume). 把数据文件布置在系统上一个已知位置,使得主机上的工具应用容易访问 . 缺点是需用户确保目录的存在, 目录权限,及其他的主机上一些安全机制配置正确. 


1.	主机上创建数据目录, 如 `/my/own/datadir`.
2.	启动 `redmine` 容器:

	```console
	$ docker run -d --name some-redmine -v /my/own/datadir:/usr/src/redmine/files --link some-postgres:postgres redmine
	```

命令`-v /my/own/datadir:/usr/src/redmine/files` 的这部分会把底层主机系统的 `/my/own/datadir` 目录 挂载进入容器的 `/usr/src/redmine/files` , 此处Redmine 会存储上传的文件.

注意主机selinux policy 使能的用户,此处可能有问题. 当前的解决方法是给新的数据目录分配相关的selinux policy ,这样就能允许容器访问:

```console
$ chcon -Rt svirt_sandbox_file_t /my/own/datadir
```

### 端口映射

不了解容器的ip, 访问redmine , 可以使用标准的端口映射方式. 给 `docker run`  添加参数 `-p 3000:3000`, 然后打开浏览器访问 `http://localhost:3000` 或者 `http://host-ip:3000`

### 环境变量

启动 `redmine` 容器时 ,可通过给 `docker run` 命令传递多种环境变量来调节redmine 实例配置. 

#### `REDMINE_NO_DB_MIGRATE`

这个变量允许你控制是否在容器启动时运行 `rake db:migrate`. 把此变量设置成像 `1`, `true` 这样的非空字符串,迁移脚本在容器启动时就不会自动运行. 

当你使用非容器默认 `CMD`(比如 `bash`)启动镜像,  `db:migrate` 也不会运行 . 细节请看镜像入口点`docker-entrypoint.sh`.

#### `REDMINE_SECRET_KEY_BASE`

这个变量用于创建初始的 `config/secrets.yml`文件,同时设置 `secret_key_base` 值.  此值被rails 用来编码cookie ,存储会话数据以防止篡改. ([session store](https://www.redmine.org/projects/redmine/wiki/RedmineInstall#Step-5-Session-store-secret-generation)). 如果不设置此变量或提供`secrets.yml`, 会使用 `rake generate_secret_token` 自动生成一个.

