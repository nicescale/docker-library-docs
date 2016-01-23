# 支持的tag标签，以及相关`Dockerfile`链接

-	[`2.3.1`, `latest` (*2.3.1/Dockerfile*)](https://github.com/neo4j/docker-neo4j/blob/026fcee712c5fd2596c7d41e14c2981404d94df2/2.3.1/Dockerfile)
-	[`2.3.1-enterprise`, `enterprise` (*2.3.1-enterprise/Dockerfile*)](https://github.com/neo4j/docker-neo4j/blob/026fcee712c5fd2596c7d41e14c2981404d94df2/2.3.1-enterprise/Dockerfile)

关于本镜像的更详细信息，请访问：[清单文件 (`library/neo4j`)](https://github.com/docker-library/official-images/blob/master/library/neo4j). 镜像更新依赖于 [`docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

关于镜像每个layer以及上述每个tag的详细信息，请查看位于[`docker-library/docs` GitHub repo](https://github.com/docker-library/docs)的 [`neo4j/tag-details.md`](https://github.com/docker-library/docs/blob/master/neo4j/tag-details.md)文件。

# 关于Neo4j

Neo4j是一个高性能的NOSQL图形数据库. 被大量用于公司和政府的关键应用。你可以在[这里](http://neo4j.com/developer)了解更多。

![logo](https://raw.githubusercontent.com/docker-library/docs/master/neo4j/logo.png)

# 镜像的使用

*注意:* Docker不能直接在OSX和Windows上运行，有这个情况可以在虚拟机中运行。你可以参考Docker的稳定([OSX](http://docs.docker.com/engine/installation/mac/), [Windows](http://docs.docker.com/engine/installation/windows/)). The instructions below include variants to cope with the difference between platforms where necessary; they assume that on OSX you have a shell set up as per the instructions linked above and that you have a Docker machine VM called `default`.

The image exposes two ports (`7474` and `7473`) for HTTP and HTTPS access to the Neo4j API and a volume (`/data`) to allow the database to be persisted outside its container.

	docker run \
	    --detach \
	    --publish=7474:7474 \
	    --volume=$HOME/neo4j/data:/data \
	    neo4j

Point your browser at `http://localhost:7474` on Linux or `http://$(docker-machine ip default):7474` on OSX.

*NOTE:* All the volumes in this guide are stored under `$HOME` in order to work on OSX where `$HOME` is automatically mounted into the machine VM. On Linux the volumes can be stored anywhere.

*NOTE:* By default Neo4j requires authentication. You have to login with `neo4j/neo4j` at the first connection and set a new password.

## Neo4j 版本

Neo4j 有两种版本: 普通版和企业版.

Neo4j 企业版是专为商业部署,更注重规模和可用性。 使用Neo4j企业版需要拥有Neo Technology公司的商业许可协议. 详见 [Neo4j licensing](http://neo4j.com/licensing/)。

标签包含两个版本. 特定企业版标签有一个企业版后缀 `-enterprise`(比如 `neo4j:2.3.0-enterprise`), 普通版标签没有后缀(如 `neo4j:2.3.0`). 最新的企业版是`neo4j:enterprise`.

## Docker 配置

### 文件描述符的限制

如果使用了大量索引或有大量并发的数据库连接，Neo4j可能会使用大量文件描述符。

Docker 可以控制在一个容器中打开的文件描述符的数量; 其极限取决于系统的配置。我们建议运行Neo4j只是需要40000。

运行以下命令，测试限制:

	docker run neo4j \
	    bash -c 'echo Soft limit: $(ulimit -Sn); echo Hard limit: $(ulimit -Hn)'

使用默认配置, 可以使用 `--ulimit` 参数:

	docker run \
	    --detach \
	    --publish=7474:7474 \
	    --volume=$HOME/neo4j/data:/data \
	    --ulimit=nofile=40000:40000
	    neo4j

## Neo4j 配置

该镜像提供了缺省配置，以便于了解 Neo4j, 但在生产使用使用时必须修改使它 ，尤其当允许多个容器运行在同一台服务器上时的内存分配的极其有限(参考前文的`NEO4J_CACHE_MEMORY` 和 `NEO4J_HEAP_MEMORY`)。 你可以通过[manual](http://neo4j.com/docs/stable/configuration.html)了解更多的Neo4j配置信息。

这里有三种不同的修改配置的方式。

### 环境变量

当运行时，通过环境变量传递到容器。

	docker run \
	    --detach \
	    --publish=7474:7474 \
	    --volume=$HOME/neo4j/data:/data \
	    --env=NEO4J_CACHE_MEMORY=4G \
	    neo4j

以下是可用的环境变量:

-	`NEO4J_CACHE_MEMORY`: Neo4j主机内存大小, 默认为 512M
-	`NEO4J_HEAP_MEMORY`: Neo4j堆的大小（MB），默认为 512
-	`NEO4J_KEEP_LOGICAL_LOGS`: 为逻辑日志保留的大小, 默认为 `100M size`
-	`NEO4J_AUTH`: 控制认证, 设置 `none` 为禁用验证； `neo4j/<password>`设置默认密码，(参考[这里](http://neo4j.com/docs/stable/rest-api-security.html))
-	`NEO4J_THIRDPARTY_JAXRS_CLASSES`: 非托管扩展的 URI mappings  (见下文)

#### 企业版

以下设置控制功能仅可在Neo4j的企业版中使用。

-	`NEO4J_DATABASE_MODE`: 数据库模式, 默认为 `SINGLE`, 设置 `HA` 可创建集群
-	`NEO4J_SERVER_ID`: 服务的id, 必须包含在一个集群中。
-	`NEO4J_HA_ADDRESS`: 集群中HA模式的主机地址，必须集群下的每个服务设置。
-	`NEO4J_INITIAL_HOSTS`: 其他集群成员，逗号分隔。

### `/conf` 卷

为容器提供包含任意配置的`/conf` 卷.

	docker run \
	    --detach \
	    --publish=7474:7474 \
	    --volume=$HOME/neo4j/data:/data \
	    --volume=$HOME/neo4j/conf:/conf \
	    neo4j

`/conf`卷下的任何配置文件会被覆盖到镜像中. 这些变量可以通过Docker环境变量来设置 。所以你要改变一个配置，需要保证其余的配置是完整和正确的。

使用`dump-config` 命令转储配置文件.

	docker run --rm\
	    --volume=$HOME/neo4j/conf:/conf \
	    neo4j dump-config

### 构建一个新的镜像

通过这个，你可以创建一个复杂的镜像.

	FROM neo4j

## Neo4j HA

(此功能只能在Neo4j企业版.)

为了运行Neo4j HA模式，Docker需要连接在集群中的其他容器,使他们可以互相通信. 每个容器都要有连接其它节点的网络路由， `NEO4J_HA_ADDRESS` 和 `NEO4J_INITIAL_HOSTS` 环境变量都需要设置(如前文所述).

仅有一个Docker主机, 可以如下设置.

	docker network create --driver=bridge cluster
	
	docker run --name=instance1 --detach --publish=7474:7474 --net=cluster --hostname=instance1 \
	    --env=NEO4J_DATABASE_MODE=HA --env=NEO4J_HA_ADDRESS=instance1 --env=NEO4J_SERVER_ID=1 \
	    --env=NEO4J_INITIAL_HOSTS=instance1:5001,instance2:5001,instance3:5001 \
	    neo4j:enterprise
	
	docker run --name=instance2 --detach --publish 7475:7474 --net=cluster --hostname=instance2 \
	    --env=NEO4J_DATABASE_MODE=HA --env=NEO4J_HA_ADDRESS=instance2 --env=NEO4J_SERVER_ID=2 \
	    --env=NEO4J_INITIAL_HOSTS=instance1:5001,instance2:5001,instance3:5001 \
	    neo4j:enterprise
	
	docker run --name=instance3 --detach --publish 7476:7474 --net=cluster --hostname=instance3 \
	    --env=NEO4J_DATABASE_MODE=HA --env=NEO4J_HA_ADDRESS=instance3 --env=NEO4J_SERVER_ID=3 \
	    --env=NEO4J_INITIAL_HOSTS=instance1:5001,instance2:5001,instance3:5001 \
	    neo4j:enterprise

## 插件和非托管扩展

通过提供包含jars的 `/plugins` 卷可以安装插件或非托管扩展. 对于非托管扩展还需要配置一个环境变量指定 URI map.

	docker run --publish 7474:7474 --volume=$HOME/neo4j/plugins:/plugins \
	    --env=NEO4J_THIRDPARTY_JAXRS_CLASSES=com.example.extension=/example
	    neo4j

See the [manual](http://neo4j.com/docs/stable/server-extending.html) for more details on plugins and unmanaged extensions.

## Neo4j shell

可以使用这样的命令在本地的一个容器中运行Neo4j shell:

	docker exec --interactive <container> bin/neo4j-shell

# 支持的 docker 版本

此镜像支持 docker 官方版本 1.9.1.

或者更老的版本 (到1.6).

请参考 [Docker安装文档](https://docs.docker.com/installation/)了解如果升级docker daemon


