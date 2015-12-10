# 支持的镜像tag 和 `Dockerfile` 链接

-	[`6-jre-1.7.2-jaxrs` (*6-jre-1.7.2-jaxrs/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/6-jre-1.7.2-jaxrs/Dockerfile)
-	[`6-jre-1.7.2-plume` (*6-jre-1.7.2-plume/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/6-jre-1.7.2-plume/Dockerfile)
-	[`6-jre-1.7.2-plus` (*6-jre-1.7.2-plus/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/6-jre-1.7.2-plus/Dockerfile)
-	[`6-jre-1.7.2-webprofile` (*6-jre-1.7.2-webprofile/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/6-jre-1.7.2-webprofile/Dockerfile)
-	[`7-jre-1.7.2-jaxrs` (*7-jre-1.7.2-jaxrs/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/7-jre-1.7.2-jaxrs/Dockerfile)
-	[`7-jre-1.7.2-plume` (*7-jre-1.7.2-plume/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/7-jre-1.7.2-plume/Dockerfile)
-	[`7-jre-1.7.2-plus` (*7-jre-1.7.2-plus/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/7-jre-1.7.2-plus/Dockerfile)
-	[`7-jre-1.7.2-webprofile` (*7-jre-1.7.2-webprofile/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/7-jre-1.7.2-webprofile/Dockerfile)
-	[`8-jre-1.7.2-jaxrs` (*8-jre-1.7.2-jaxrs/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/8-jre-1.7.2-jaxrs/Dockerfile)
-	[`8-jre-1.7.2-plume` (*8-jre-1.7.2-plume/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/8-jre-1.7.2-plume/Dockerfile)
-	[`8-jre-1.7.2-plus` (*8-jre-1.7.2-plus/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/8-jre-1.7.2-plus/Dockerfile)
-	[`8-jre-1.7.2-webprofile` (*8-jre-1.7.2-webprofile/Dockerfile*)](https://github.com/tomitribe/docker-tomee/blob/986e80301ac11cfaa7bf5554ff2516badcda8d96/8-jre-1.7.2-webprofile/Dockerfile)

# 什么是TomEE?

Apache TomEE,读作"Tommy",是一个全部来自apache通过java ee web认证过的软件栈,其中tomcat是其核心组件.TomEE是由原版apache tomcat zip 文件组装而成.从tomcat开始,添加了我们的jars,然后把其他的压缩进来,结果就是tomcat 添加了EE特性-TomEE. 

![logo](https://raw.githubusercontent.com/docker-library/docs/master/tomee/logo.png)

Apache TomEE 有四种不同模式: Web Profile, JAX-RS, Plus and Plume.

-	Apache TomEE Web Profile 提供了 Servlets, JSP, JSF, JTA, JPA, CDI, Bean Validation and EJB Lite.
-	Apache TomEE JAX-RS (RESTfull Services) 提供了 the Web Profile plus JAX-RS (RESTfull Services).
-	Apache TomEE Plus 提供了所有Web Profile和JAX-RS 的功能(RESTfull Services), 加上 EJB Full, Java EE Connector Architecture, JMS (Java Message Service) and JAX-WS (Web Services).
-	Apache TomEE Plume 提供了所有Plus Profile 功能, 另外引入 Mojarra and EclipseLink 支持.

所有支持版本的dockerfile 文件在这里找到 `https://github.com/tomitribe/docker-tomee`.

# Apache TomEE and Tomitribe

Tomitribe 为TomEE提供商业支持,专业服务及相关培训.我们提供了新的商业模式,进一步推进开发和开源项目的增长,满足产品支持的业务要求.Tomitribe社区参与项目为商业和社区提供沟通.

所有Tomitribe开发者都是TomEE代码提交者和项目开发方向的引导者.我们想对相关的人,公司,TomEE用户扩大这种影响.如何做到?Tomitribe社区参与项目.

# 怎么使用镜像

运行默认的 TomEE server (`CMD ["catalina.sh", "run"]`):

	docker run -it --rm tomee:<java-version>\-<tomeeversion>\-<flavour>

例如运行Apache TomEE 1.7.2 结合 JRE 8 和 Webprofile:

	docker run -it --rm tomee:8-jre-1.7.2-webprofile

浏览器里打开 `http://container-ip:8080`测试它,主机外访问端口8888:

	docker run -it --rm -p 8888:8080 tomee:<java-version>\-<tomeeversion>\-<flavour>

浏览器里打开 `http://localhost:8888` 或者 `http://host-ip:8888`.

配置文件在 `/usr/local/tomee/conf/`. 默认, 要求操作"/manager/html" web应用的"manager-gui"角色下没有用户.如果你想运行此应用,必须在`tomcat-users.xml`中定义这样的用户.

你可以此镜像为基础来部署你的war应用.为了做到这个,基于Tomee镜像创建Dockerfile,把war文件添加到`webapp`目录:

	ADD <locationofapplication>/<warfile> /usr/local/tomee/webapps/<warfile>

