# 支持的镜像tag 和 `Dockerfile` 链接

-	[`latest`, `5.2` (*5.2/Dockerfile*)](https://github.com/SonarSource/docker-sonarqube/blob/2f7cc2f6ef7f0206b697c37df09fe2e0fd36c8f4/5.2/Dockerfile)
-	[`lts`, `4.5.6` (*4.5.6/Dockerfile*)](https://github.com/SonarSource/docker-sonarqube/blob/2f7cc2f6ef7f0206b697c37df09fe2e0fd36c8f4/4.5.6/Dockerfile)

# 什么是SonarQube?

SonarQube 是持续检查代码质量的开源工具.

> [wikipedia.org/wiki/SonarQube](http://en.wikipedia.org/wiki/SonarQube)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/sonarqube/logo.png)

# 怎么使用镜像

## 运行 SonarQube

像下面这样启动SonarQube:

```console
$ docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube:5.1
```

开始分析一个项目:

```console
$ On Linux:
mvn sonar:sonar

$ With boot2docker:
mvn sonar:sonar -Dsonar.host.url=http://$(boot2docker ip):9000 -Dsonar.jdbc.url="jdbc:h2:tcp://$(boot2docker ip)/sonar"
```

## 数据库配置

默认, 镜像使用了不太适合产品部署的嵌入式数据库H2.

产品数据库的配置使用这些环境变量: `SONARQUBE_JDBC_USERNAME`, `SONARQUBE_JDBC_PASSWORD`, `SONARQUBE_JDBC_URL`.

```console
$ docker run -d --name sonarqube \
	-p 9000:9000 -p 9092:9092 \
	-e SONARQUBE_JDBC_USERNAME=sonar \
	-e SONARQUBE_JDBC_PASSWORD=sonar \
	-e SONARQUBE_JDBC_URL=jdbc:postgresql://localhost/sonar \
	sonarqube:5.1
```

更多的sonar知识 [here](https://github.com/SonarSource/docker-sonarqube/blob/master/recipes.md).

## 管理

管理指南[here](http://docs.sonarqube.org/display/SONAR/Administration+Guide).
