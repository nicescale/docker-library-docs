# 支持的镜像tag 和 `Dockerfile` 链接

-	[`kernel`, `8.5.5.7-kernel` (*websphere-liberty/8.5.5/developer/kernel/Dockerfile*)](https://github.com/WASdev/ci.docker/blob/975e98caeaf81eb0648832d54778e0d6b079e4b4/websphere-liberty/8.5.5/developer/kernel/Dockerfile)
-	[`common`, `8.5.5.7-common` (*websphere-liberty/8.5.5/developer/common/Dockerfile*)](https://github.com/WASdev/ci.docker/blob/975e98caeaf81eb0648832d54778e0d6b079e4b4/websphere-liberty/8.5.5/developer/common/Dockerfile)
-	[`webProfile6`, `8.5.5.7-webProfile6` (*websphere-liberty/8.5.5/developer/webProfile6/Dockerfile*)](https://github.com/WASdev/ci.docker/blob/975e98caeaf81eb0648832d54778e0d6b079e4b4/websphere-liberty/8.5.5/developer/webProfile6/Dockerfile)
-	[`webProfile7`, `8.5.5.7-webProfile7` (*websphere-liberty/8.5.5/developer/webProfile7/Dockerfile*)](https://github.com/WASdev/ci.docker/blob/975e98caeaf81eb0648832d54778e0d6b079e4b4/websphere-liberty/8.5.5/developer/webProfile7/Dockerfile)
-	[`javaee7`, `8.5.5.7-javaee7`, `8.5.5.7`, `8.5.5`, `latest` (*websphere-liberty/8.5.5/developer/javaee7/Dockerfile*)](https://github.com/WASdev/ci.docker/blob/975e98caeaf81eb0648832d54778e0d6b079e4b4/websphere-liberty/8.5.5/developer/javaee7/Dockerfile)
-	[`beta` (*websphere-liberty/beta/Dockerfile*)](https://github.com/WASdev/ci.docker/blob/fa7b9fb3f682edb289c77f065e7145efd910a18d/websphere-liberty/beta/Dockerfile)

# 纵观

本仓库中的镜像包括IBM WebSphere 应用服务器和IBM Java 运行时环境.更多产看[WASdev](https://developer.ibm.com/wasdev/docs/category/getting-started/)

# 镜像

此仓库包含多个镜像版本.`beta` tag 包含最新的每月beta 版本JAR运行时.其他镜像都是基于最近的修护版本.

`kernel` tag 仅包含liberty 核心,没有其他的额外的运行时特性.此版本可用于基础镜像,为用户定制特定应用需要的特性构建镜像.例如,下面的Dockerfile启动此镜像,往`server.xml`里拷贝应用需要的特性列表,然后使用`installUtility`命令从在线仓库下载那些特性.

```dockerfile
FROM websphere-liberty:kernel
COPY server.xml /opt/ibm/wlp/usr/servers/defaultServer/
RUN installUtility install --acceptLicense defaultServer
```

`common` tag 镜像添加一些产品环境下期待的特性.其中包括如下:`appSecurity-2.0`, `collectiveMember-1.0`, `localConnector-1.0`, `ldapRegistry-3.0`, `monitor-1.0`, `requestTiming-1.0`, `restConnector-1.0`, `sessionDatabase-1.0`, `ssl-1.0` and `webCache-1.0`.此镜像作为`webProfile6`和`webProfile7`镜像的基础.

`webProfile6`镜像包含Java EE6 Web Profile compliance要求的一些特性.它同样包含一些与运行时JAR一致的额外特性,其中最为显著的是运行OSGI应用所要求的.

`webProfile7`镜像包含Java EE7 Web Profile compliance要求的一些特性.`javaee7`拓展了此镜像,添加了一些JavaEE7完整的平台兼容要求的特性.`javaee7`镜像也被标记为`latest`.

同样存在与Liberty发布版本对应的镜像tag, 它们主要用于指示当前使用中的镜像对应哪个版本的Liberty,而且在后续版本可用时,这些tag会被新的替换掉.所以,仅在你明确要求新发布可用时,在旧版本上构建失败时,才使用这些tag的镜像.Liberty 零迁移策略,保证使用无特定版本tag镜像的应用, 在新版本可用时,能继续正常工作.

# 使用

使用本仓库的任何镜像,都需要用户接受WebSphere Application Server for Developers和IBM JRE许可.启动镜像时,设置环境变量`LICENSE`值为`accept`.通过把此变量设置`view`来查看许可条款.失败的设置,会导致容器终止.

这些镜像被设计成支持几种不同的使用模式.下面的例子基于Java EE6 Liberty[application deployment sample](https://developer.ibm.com/wasdev/docs/article_appdeployment/)假设[DefaultServletEngine.zip](https://www.ibm.com/developerworks/mydeveloperworks/blogs/wasdev/resource/DefaultServletEngine.zip)已经被解压到`/tmp`,同时`server.xml`被更新,允许接收容器外部的http连接,可通过在`server`节添加下面元素做到:

```xml
<httpEndpoint host="*" httpPort="9080" httpsPort="-1"/>
```

1.	每个镜像都包含了指定相应特性的默认server 配置,http,https 分别使用9080,9443端口.可以把WAR文件挂载到容器内的`dropins`目录,启动运行server.下面的例子, 以后台方式启动容器,挂载了主机上的WAR文件,把HTTP,HTTPS分别映射到主机上80,443 端口上. 

	```console
	$ docker run -e LICENSE=accept -d -p 80:9080 -p 443:9443 \
	    -v /tmp/DefaultServletEngine/dropins/Sample1.war:/opt/ibm/wlp/usr/servers/defaultServer/dropins/Sample1.war \
	    websphere-liberty:webProfile6
	```

	一旦server启动, 打开浏览器访问 http://localhost/Sample1/SimpleServlet .

	注意: 如果你在osx,windows上使用boot2docker,使用 `boot2docker ip`命令获取虚拟主机的ip替代localhost.

2.	为了配置上的极大灵活性,可把主机上的server配置目录整个挂入容器内部,同时指定server名字作为参数来启动容器. 注意,这个特定的例子仅配置了HTTP访问.

	```console
	$ docker run -e LICENSE=accept -d -p 80:9080 \
	  -v /tmp/DefaultServletEngine:/opt/ibm/wlp/usr/servers/DefaultServletEngine \
	  websphere-liberty:webProfile6 /opt/ibm/wlp/bin/server run DefaultServletEngine
	```

3.	也可以此镜像为基础构建自己的应用层,可选择使用默认server配置或新的server配置,可在构建是接受许可.这里我们已从`/tmp/DefaultServletEngine/dropins`拷贝`Sample1.war`到下面Dockerfile相同的目录. 

	```dockerfile
	FROM websphere-liberty:webProfile6
	ADD Sample1.war /opt/ibm/wlp/usr/servers/defaultServer/dropins/
	ENV LICENSE accept
	```

	This can then be built and run as follows:

	```console
	$ docker build -t app .
	$ docker run -d -p 80:9080 -p 443:9443 app
	```

4.	最后, 往容器上挂载包含应用和服务器配置文件的数据卷容器,是可能的. 这样,没有主机文件的依赖,允许应用容器很容易重新挂载一个新版本的应用服务器镜像.样例,假设你已经把`/tmp/DefaultServletEngine` 目录拷贝到Dokcerfile所在的目录中.

	构建运行数据卷容器:

	```dockerfile
	FROM websphere-liberty:webProfile6
	ADD DefaultServletEngine /opt/ibm/wlp/usr/servers/DefaultServletEngine
	```

	```console
	$ docker build -t app-image .
	$ docker run -d -v /opt/ibm/wlp/usr/servers/DefaultServletEngine \
	    --name app app-image true
	```

	使用挂载来自数据容器的卷, 启动WebSphere Liberty:

	```console
	$ docker run -e LICENSE=accept -d -p 80:9080 \
	  --volumes-from app websphere-liberty:webProfile6 \
	  /opt/ibm/wlp/bin/server run DefaultServletEngine
	```

