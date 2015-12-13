# 支持的tag和其 `Dockerfile` 链接地址

-	[`5.5`, `7.0-php5.5`, `7.0`, `latest` (*7.0/5.5/Dockerfile*)](https://github.com/zendtech/php-zendserver/blob/bb3303d34a3e86a4bc9eb1d652c5cce6ad441a6e/7.0/5.5/Dockerfile)
-	[`5.4`, `7.0-php5.4` (*7.0/5.4/Dockerfile*)](https://github.com/zendtech/php-zendserver/blob/bb3303d34a3e86a4bc9eb1d652c5cce6ad441a6e/7.0/5.4/Dockerfile)

关于这个镜像的更多历史信息请查看[the relevant manifest file (`library/php-zendserver`)](https://github.com/docker-library/official-images/blob/master/library/php-zendserver). 这个镜像通过[the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images)拉取更新.

关于上面所说支持的tag实际大小/传输大小和每个Layers的详细信息, 请查看 [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs)下面的[the `perl/tag-details.md` file](https://github.com/docker-library/docs/blob/master/perl/tag-details.md).

# 什么是 Zend Server?

Zend Server是一个PHP mobile 和 web apps的完整应用平台. Zend Server提供了一个高度可用的PHP生产环境，其中包括在其他功能中一个高可靠的PHP栈, 应用监测,故障排除和全新的Z-Ray. ###使用带有Z-Ray的Zend Server提高你的开发效率,不费力地给开发者深入了解他们的代码是如何运行的，因为它们正在开发它，所有不需要改变他们的习惯或工作流程. 用Z-Ray, 开发者可以立即了解自己的代码变化的影响，使他们能够提高质量. 除了明显的好处 ‘Left Shifting’ – 更好的性能, 更少的生产问题和更快的恢复时间 – 使用 Z-Ray 也是非常有趣的! ###推动持续交付Zend Server平台,在整个应用程序交付周期持续交付,提供一致性、自动化和协作能力. 可整合: Chef, Jenkins, Nagios, Vmware, Puppet.

###额外的资源 http://www.zend.com/ http://kb.zend.com/ http://files.zend.com/help/Zend-Server/zend-server.htm#faqs.htm http://files.zend.com/help/Zend-Server/zend-server.htm#getting_started.htm

# PHP-ZendServer

这是一个 Dockerized Zend Server 7.0容器的集群版本. Docker上的Zend Server,你会得到你的PHP应用程序启动和运行的高可用性PHP生产环境，其中包括在其他功能中一个高可靠的PHP栈, 应用监测,故障排除,和新技术-Z-Ray. Z-Ray通过跟踪和显示在工具栏上的详细信息，为开发人员提供了前所未有的可视化功能，详细介绍了如何构建页面的各种元素的详细信息.

## 使用

#### 从Docker-Hub运行容器

Zend Server共享在[Docker-Hub](https://registry.hub.docker.com/_/php-zendserver/) as **php-zendserver**. -
启动一个单独的Zend Server实例，执行:

	    $ docker run php-zendserver

-	您可以通过添加指定的PHP和Zend Server版本 ':<php-version>' 或 ':&lt;ZS-version&gt;-php&lt;version&gt;' 去运行 'docker run' 命令. 可得到 PHP 版本是 5.4 & 5.5 (5.5 is the default) 和 Zend Server 7 (例如: php-zendserver:7.0-php5.4).

- 启动Zend Server集群,每个节点都执行以下命令:

	```console
	$ docker run -e MYSQL_HOSTNAME=<db-ip> -e MYSQL_PORT=3306 -e MYSQL_USERNAME=<username> -e MYSQL_PASSWORD=<password> -e MYSQL_DBNAME=zend php-zendserver
	```

#### 从Dockerfile运行容器

- 从一个包含它克隆的本地文件夹运行以下命令生成镜像. **image-id** 将被输出:

	```console
	$ docker build .
	```

- 启动一个单独的Zend Server实例, 执行:

	```console
	$ docker run <image-id>
	```

-	启动 Zend Server 集群, 每个节点执行以下命令:

	```console
	$ docker run -e MYSQL_HOSTNAME=<db-ip> -e MYSQL_PORT=3306 -e MYSQL_USERNAME=<username> -e MYSQL_PASSWORD=<password> -e MYSQL_DBNAME=zend <image-id>
	```

#### 访问 Zend server

一旦启动, 容器将输出所需信息去访问PHP应用程序并且Zend Server UI会包含自动生成的管理员密码.

访问容器 **remotely**, 必须配置端口转发, 手动或者使用docker. 例如, 这个命令将重新指定80端口到88端口, 和10081(Zend Server UI 端口)端口到10088端口:

	    $ docker run -p 88:80 -p 10088:10081 php-zendserver

对于集群实例:

	    $ docker run -p 88:80 -p 10088:10081 -e MYSQL_HOSTNAME=<db-ip> -e MYSQL_PORT=3306 -e MYSQL_USERNAME=<username> -e MYSQL_PASSWORD=<password> -e MYSQL_DBNAME=zend <image-id>

请注意，在运行多个实例时只有一个实例可以绑定到一个端口. 如果你正在运行一个集群，可以指定一个端口重定向到一个节点，或指定一个不同的端口到每个容器.

#### 环境变量

环境变量通过运行"-e"命令切换

##### 可选的环境变量:

指定一个预定义的管理员密码使用Zend服务器: - ZS_ADMIN_PASSWORD

集群的MySQL变量. *所有* 需要将节点正确的加入集群: - MYSQL_HOSTNAME - MySQL 数据库的IP和主机名 - MYSQL_PORT - MySQL 监听端口 - MYSQL_USERNAME - MYSQL_PASSWORD - MYSQL_DBNAME - Zend Server将使用数据库名字 (如果不存在会自动创建).

要指定一个预先购买的许可证使用下面的环境变量: - ZEND_LICENSE_KEY - ZEND_LICENSE_ORDER

### 最低要求

-	每个Zend Server Docker 容器需要1GB内存.

# 许可证

[Zend Technologies Ltd.](https://www.zend.com/topics/License-EULA-2010-09-2.pdf)

# 支持的docker版本

此镜像官方支持docker1.9.1

老版本(往下到1.6)提供最基础的支持

请查看 [Docker安装文档](https://docs.docker.com/installation/) 来获取你的docker升级详情.


# 用户反馈

我们错过什么了吗？请告诉我们: http://www.zend.com/en/support-center
