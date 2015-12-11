# 什么是 Bonita BPM?

Bonita BPM 是开源的公司业务流程和工作流的解决方案，创建于2001年。最初由法国国家信息与自动化研究所(INRIA) 创建，之后由法国 Bull 集团内部进行孵化。自2009年起，由 Bonitasoft 软件公司全力进行 Bonita 的研发。

> [wikipedia.org/wiki/Bonita_BPM](http://en.wikipedia.org/wiki/Bonita_BPM)

![logo](https://raw.githubusercontent.com/Bonitasoft-Community/docker_docs/master/bonita/logo.png)

# 如何使用此镜像

## 快速开始

	docker run --name bonita -d -p 8080:8080 bonita

这将会启动一个运行了包含了 Bonita BPM Engine + Bonita BPM Portal 的[Tomcat Bundle](http://documentation.bonitasoft.com/tomcat-bundle-2) 的容器。因为这里没有设置任何环境变量，意味着你使用了 bonita bunlde 默认的启动脚本(包含了对 REST API 的安全优化)，并默认使用了 H2 数据库。

你可以从 http://localhost:8080/bontina 访问 Bonita BPM Portal，使用默认帐号密码: install /install 登录。

## 将 Bonita BPM 和数据库连接

### PostgreSQL

PostgreSQL 是推荐使用的数据库

[修改 max_prepared_transactions 配置](http://documentation.bonitasoft.com/database-configuration-business-data-1):

	mkdir -p custom_postgres
	echo '#!/bin/bash' > custom_postgres/bonita.sh
	echo 'sed -i "s/^.*max_prepared_transactions\s*=\s*\(.*\)$/max_prepared_transactions = 100/" "$PGDATA"/postgresql.conf' >> custom_postgres/bonita.sh
	chmod +x custom_postgres/bonita.sh

将此目录映射到 PostgreSQL 的 /docker-entrypoint-initdb.d 目录

	docker run --name mydbpostgres -v "$PWD"/custom_postgres/:/docker-entrypoint-initdb.d -e POSTGRES_PASSWORD=mysecretpassword -d postgres:9.3

参考 [官方PostgreSQL 文档](https://registry.hub.docker.com/_/postgres/) 获取更多信息

	docker run --name bonita_postgres --link mydbpostgres:postgres -d -p 8080:8080 bonita

### MySQL


MySQL 的存储引擎在管理 XA transactions 事务时有一些 bug: 详见 [17343](http://bugs.mysql.com/bug.php?id=17343) 和 [12161](http://bugs.mysql.com/bug.php?id=12161)。因此，并不是很推荐在生产环境中使用 MySQL 作为 Bonita 的数据库。

[修改 max_allowed_packet 配置](http://documentation.bonitasoft.com/database-configuration-2#mysqlspec)，默认的配置 1M 不是很合适:

	mkdir -p custom_mysql
	echo "[mysqld]" > custom_mysql/bonita.cnf
	echo "max_allowed_packet=16M" >> custom_mysql/bonita.cnf

将这个目录映射到 MySQL 容器的 /etc/mysql/conf.d 目录

	docker run --name mydbmysql -v "$PWD"/custom_mysql/:/etc/mysql/conf.d -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql:5.5

参考[官方MySQL文档](https://registry.hub.docker.com/_/mysql/) 获取更多信息

运行应用容器，并将其连接到 MySQL 容器:

	docker run --name bonita_mysql --link mydbmysql:mysql -d -p 8080:8080 bonita

## 修改默认账号

	docker run --name=bonita -e "TENANT_LOGIN=tech_user" -e "TENANT_PASSWORD=secret" -e "PLATFORM_LOGIN=pfadmin" -e "PLATFORM_PASSWORD=pfsecret" -d -p 8080:8080 bonita

你将可以通过 localhost:8080/bonita 访问 Bonita BPM Portal，并使用以下帐号登录: tech_user / secret

## 如何存储数据

如果你使用上述的方式，将 Bonita 容器和 PostgreSQL 和 MySQL 容器连接，那么你的大部分数据就已经和 Bonita 容器分开存储了。然后还有一部分数据存储在 Bonita bundle 中。Bonita Home 的数据目录是 `bonita`, 包含了配置，工作流和临时的目录和文件，另外还有一些日志文件位于 `logs` 目录。

重要提示: 存储多少方式来存储运行在容器中的应用的数据，我们推荐用户使用 `bonita` 镜像来了解以下几种方式:


-	使用 Docker 自己的 [数据卷](https://docs.docker.com/userguide/dockervolumes/#adding-a-data-volume). 这是默认而且对用户来说最简单透明的方式。不好的地方对于直接运行在主机上的应用，不能很轻易地定位和访问数据目录
-	在主机上创建数据目录并将其[映射到容器](https://docs.docker.com/userguide/dockervolumes/#mount-a-host-directory-as-a-data-volume). 这将使容器直接使用主机上的固定目录来存储数据 ，方便主机上的其它工具或者应用访问到这个目录。不方便的地方是，用户需要自己确认这个目录是否存在，并且有正确的权限设置使得容器中的进程能够访问这个目录下的数据。

通过上述链接到的两个 Docker 文档可以详细了解不同的数据存储方式。这里我们简单地对第二种使用方式进行介绍:

1.	在主机的合适位置创建一个数据目录，比如 `/my/own/datadir`
2.	使用以下命令运行你的 `bonita` 容器:

	docker run --name some-bonita -v /my/own/datadir:/opt/bonita -d bonita:tag

这里通过 `-v /my/own/datadir:/opt/bonita` 参数将 `/my/own/datadir` 从主机映射到容器的 `/opt/bonita` 目录，容器的 `/opt/bonita` 目录是 Bontina bundle 部署和操作数据的默认目录。

需要注意的是如果主机上启用了 SELinux，直接这样使用可能会遇到一些问题。解决办法是为这个目录增加一个 SELinux policy 设置，使得容器可以有权限访问这个目录:

	chcon -Rt svirt_sandbox_file_t /my/own/datadir

## 从早期版本的 Bonita BPM 迁移数据

1.	停止容器

		docker stop bonita_7.0.0_postgres

2.	检查数据的存储位置

		docker inspect bonita_7.0.0_postgres
		[...]
		    "Mounts": [
		        {
		            "Source": "/home/user/Documents/Docker/Volumes/bonita_7.0.0_postgres",
		            "Destination": "/opt/bonita",
		            "Mode": "",
		            "RW": true
		        }
		    ],
		[...]

3.	从文件系统中拷贝数据

		cp -r bonita_7.0.0_postgres bonita_7.0.3_postgres

4.	获取数据容器的IP地址

		docker inspect --format '{{ .NetworkSettings.IPAddress }}' mydbpostgres
		172.17.0.26

5.	备份数据库

		export PGPASSWORD=mysecretpassword
		pg_dump -O -x -h 172.17.0.26 -U postgres bonitadb > /tmp/bonitadb.sql

	Note that businessdb won't be updated with the migration tool but you may want to also backup/move it.

6.	导入数据备份

		export PGPASSWORD=mysecretpassword
		psql -U postgres -h 172.17.0.26 -d postgres -c "CREATE USER newbonitauser WITH PASSWORD 'newbonitapass';"
		psql -U postgres -h 172.17.0.26 -d postgres -c "CREATE DATABASE newbonitadb OWNER newbonitauser;"
		export PGPASSWORD=newbonitapass
		cat /tmp/bonitadb.sql | psql -U newbonitauser -h 172.17.0.26 newbonitadb

7.	下载最新版本的 migration tool 和 Bonita bundle

		cd bonita_7.0.3_postgres
		wget http://download.forge.ow2.org/bonita/bonita-migration-distrib-2.2.0.zip
		wget http://download.forge.ow2.org/bonita/BonitaBPMCommunity-7.0.3-Tomcat-7.0.55.zip
		unzip bonita-migration-distrib-2.2.0.zip -d bonita-migration-distrib-2.2.0
		unzip BonitaBPMCommunity-7.0.3-Tomcat-7.0.55.zip

8.	将之前备份的数据移到新的 Bundle 下

		mv BonitaBPMCommunity-7.0.3-Tomcat-7.0.55/bonita/ BonitaBPMCommunity-7.0.3-Tomcat-7.0.55/bonita.orig
		cp -r BonitaBPMCommunity-7.0.0-Tomcat-7.0.55/bonita/ BonitaBPMCommunity-7.0.3-Tomcat-7.0.55/bonita/

9.	配置 migration tool

		cd bonita-migration-distrib-2.2.0/

	添加 jdbc driver

		cp ../BonitaBPMCommunity-7.0.0-Tomcat-7.0.55/lib/bonita/postgresql-9.3-1102.jdbc41.jar lib/

	编辑 migration tool 配置将会其指向 bontia home 和 db 的位置

		vim Config.properties

	例如 :

		bonita.home=/home/user/Documents/Docker/Volumes/bonita_7.0.3_postgres/BonitaBPMCommunity-7.0.3-Tomcat-7.0.55/bonita
		db.vendor=postgres
		db.url=jdbc:postgresql://172.17.0.26:5432/newbonitadb
		db.driverClass=org.postgresql.Driver
		db.user=newbonitauser
		db.password=newbonitapass

10.	运行 migration 脚本

		./migration.sh

11.	运行新的容器，并指向新的 DB 和数据文件

		docker run --name=bonita_7.0.3_postgres --link mydbpostgres:postgres -e "DB_NAME=newbonitadb" -e "DB_USER=newbonitauser" -e "DB_PASS=newbonitapass" -v "$PWD"/bonita_7.0.3_postgres:/opt/bonita/ -d -p 8081:8080 bonita:7.0.3

关于 Bonita migration 的详细说明，请参考[文档](http://documentation.bonitasoft.com/migrate-earlier-version-bonita-bpm-0).

## 安全

这个镜像默认启用了静态和动态的 REST API 授权，同时关闭了 HTTP API

-	REST API 授权

	-	[Static authorization checking](http://documentation.bonitasoft.com/rest-api-authorization-0#static)

	-	[Dynamic authorization checking](http://documentation.bonitasoft.com/rest-api-authorization-0#dynamic)

-	[HTTP API](http://documentation.bonitasoft.com/rest-api-authorization-0#activate)

你可以将 HTTP_API 设置为 true 并将 REST_DYN_AUTH_CHECKS 设置为 false 来修改默认的配置:

	docker run  -e HTTP_API=true -e REST_API_DYN_AUTH_CHECKS=false --name bonita -d -p 8080:8080 bonita

## 环境变量

将你启动 `bonita` 镜像时，你可以为 `docker run` 命令添加以下的环境变量来配置 Bonita 实例。

### `PLATFORM_PASSWORD`

推荐使用这个[环境变量](http://documentation.bonitasoft.com/first-steps-after-setup-1#reset_pw)来设置 platform 的管理员密码，如何不设置这个环境变量，默认密码是 `platform`.

### `PLATFORM_LOGIN`

这个可选的环境变量用来设置 `PLATFORM_PASSWORD` 对应的 platfarm 管理员用户名。默认为 `plafromAdmin`。

### `TENANT_PASSWORD`

这个[环境变量](http://documentation.bonitasoft.com/first-steps-after-setup-1#reset_pw) 用于设置 tenant 管理员密码，默认密码是 `install`。

### `TENANT_LOGIN`

这个环境变量用于设置 `TENANT_PASSWORD` 对应的管理员用户名。默认的用户名是 `install`。

### `REST_API_DYN_AUTH_CHECKS`

这个可选的环境变量用于启用或者禁用REST API的[动态授权检查](http://documentation.bonitasoft.com/rest-api-authorization-0#dynamic)，默认值是 `true`: 启用。

### `HTTP_API`

用于启用或者禁用 Bonita HTTP API，默认值是 `false`: 禁用。

### `JAVA_OPTS`

用于修改 JAVA_OPTS，默认值是 `-Xms1024m -Xmx1024m -XX:MaxPermSize=256m`.

### `ENSURE_DB_CHECK_AND_CREATION`

用于启用或者禁止使用管理员帐号运行 SQL 语句来自动检查和创建数据库，默认是 `true`

### `DB_VENDOR`

当 Bonita 容器连接到 PostgreSQL 或者 MySQL 数据库时，这个环境变量将会自动设置成 `postgreq` 或者 `mysql`. 默认时是 `h2`，当你没有使用Docker的 `--link` 参数时，可以通过这个环境变量来修改默认值。

### `DB_HOST`, `DB_PORT`

用于将 `bonita` 镜像连接到数据库，将使用 `--link` 时，将会自动设置。

### `DB_NAME`, `DB_USER`, `DB_PASS`

这几个环境变量用于创建新用户，设置用户的密码和创建 `bonita` 数据库。

`DB_NAME` 默认值是 `bonitadb`.

`DB_USER` 默认值是 `bonitauser`.

`DB_PASS` 默认值是 `bonitapass`.

### `BIZ_DB_NAME`, `BIZ_DB_USER`, `BIZ_DB_PASS`

用于创建新的 business 数据库，用户和密码。[business 数据库](http://documentation.bonitasoft.com/business-data-model#bdmanddb).

`BIZ_DB_NAME` 默认值是 `businessdb`.

`BIZ_DB_USER` 默认值是 `businessuser`.

`BIZ_DB_PASS` 默认值是 `businesspass`.

### `DB_ADMIN_USER`, `DB_ADMIN_PASS`

用于创建数据库的管理员帐号和密码

`DB_ADMIN_USER` 如果没有设置，将会自动设置为 MySQL 的 `root` 或者 PostgreSQL 的`postgres`。

`DB_ADMIN_PASS` 如果没有设置, 将会自动从连接的数据库容器获取: `MYSQL_ENV_MYSQL_ROOT_PASSWORD` 或者 `POSTGRES_ENV_POSTGRES_PASSWORD`.

# 如何扩展镜像

如果你希望镜像进行额外的初始化，你可以添加一个 `*.sh` 脚本到 `/opt/custom-init.d` 目录。`startup.sh` 脚本将会启动服务前，运行这个目录下的任何 `*.sh` 脚本。


比如，你可以修改日志设置:

	mkdir -p custom_bonita
	echo '#!/bin/bash' > custom_bonita/bonita.sh
	echo 'sed -i "s/^org.bonitasoft.level = WARNING$/org.bonitasoft.level = FINEST/" /opt/bonita/BonitaBPMCommunity-7.0.0-Tomcat-7.0.55/conf/logging.properties' >> custom_bonita/bonita.sh
	chmod +x custom_bonita/bonita.sh
	
	docker run --name bonita_custom -v "$PWD"/custom_bonita/:/opt/custom-init.d -d -p 8080:8080 bonita

注意：有多种方式可以检查 `bonita` 的日志，其中一种方法是:

	docker exec -ti bonita_custom /bin/bash
	tail -f /opt/bonita/BonitaBPMCommunity-7.0.0-Tomcat-7.0.55/logs/bonita.`date +%Y-%m-%d`.log
