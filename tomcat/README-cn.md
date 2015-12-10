# 支持的镜像tag 和 `Dockerfile` 链接

-	[`6.0.44-jre7`, `6.0-jre7`, `6-jre7`, `6.0.44`, `6.0`, `6` (*6-jre7/Dockerfile*)](https://github.com/docker-library/tomcat/blob/3b05667011a600a2f46422dd533467eff8e7fecf/6-jre7/Dockerfile)
-	[`6.0.44-jre8`, `6.0-jre8`, `6-jre8` (*6-jre8/Dockerfile*)](https://github.com/docker-library/tomcat/blob/3b05667011a600a2f46422dd533467eff8e7fecf/6-jre8/Dockerfile)
-	[`7.0.65-jre7`, `7.0-jre7`, `7-jre7`, `7.0.65`, `7.0`, `7` (*7-jre7/Dockerfile*)](https://github.com/docker-library/tomcat/blob/8c174e038fe911fecc1a920a56afc10fd343bce2/7-jre7/Dockerfile)
-	[`7.0.65-jre8`, `7.0-jre8`, `7-jre8` (*7-jre8/Dockerfile*)](https://github.com/docker-library/tomcat/blob/8c174e038fe911fecc1a920a56afc10fd343bce2/7-jre8/Dockerfile)
-	[`8.0.29-jre7`, `8.0-jre7`, `8-jre7`, `jre7`, `8.0.29`, `8.0`, `8`, `latest` (*8-jre7/Dockerfile*)](https://github.com/docker-library/tomcat/blob/4e9726eb5182dd6fe93402485b9db4aae22177e1/8-jre7/Dockerfile)
-	[`8.0.29-jre8`, `8.0-jre8`, `8-jre8`, `jre8` (*8-jre8/Dockerfile*)](https://github.com/docker-library/tomcat/blob/4e9726eb5182dd6fe93402485b9db4aae22177e1/8-jre8/Dockerfile)

# 什么是Tomcat?

Apache Tomcat 是一个由apache软件基金会开发的开源web server和servlet container.Tomcat实现了Java Servlet和the JavaServer Pages (JSP)规范,为java代码运行提供了纯java实现http web server环境.最简单配置情况下,tomcat以单个操作系统进程运行.这个进程运行一个java虚拟机.每一个来自浏览器的http请求,是由tomcat进程的单独线程来处理.

> [wikipedia.org/wiki/Apache_Tomcat](https://en.wikipedia.org/wiki/Apache_Tomcat)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/tomcat/logo.png)Logo &copy; Apache Software Fountation

# 怎么使用镜像

运行默认tomcat服务 (`CMD ["catalina.sh", "run"]`):

```console
$ docker run -it --rm tomcat:8.0
```

浏览器里打开`http://container-ip:8080` 测试,主机外部访问端口8888:

```console
$ docker run -it --rm -p 8888:8080 tomcat:8.0
```

浏览器中访问 `http://localhost:8888` 或者 `http://host-ip:8888`.

镜像版本7,8 的默认环境变量:

	CATALINA_BASE:   /usr/local/tomcat
	CATALINA_HOME:   /usr/local/tomcat
	CATALINA_TMPDIR: /usr/local/tomcat/temp
	JRE_HOME:        /usr
	CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar

镜像版本6的环境变量:

	CATALINA_BASE:   /usr/local/tomcat
	CATALINA_HOME:   /usr/local/tomcat
	CATALINA_TMPDIR: /usr/local/tomcat/temp
	JRE_HOME:        /usr
	CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar

配置文件在 `/usr/local/tomcat/conf/`. 默认, 要求操作"/manager/html" web应用的"manager-gui"角色下没有用户.如果你想运行此应用,必须在`tomcat-users.xml`中定义这样的用户.

