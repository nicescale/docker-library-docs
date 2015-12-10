# 支持的镜像tag 和 `Dockerfile` 链接

-	[`7.7.0`, `7.7`, `7`, `latest` (*Dockerfile*)](https://github.com/getsentry/docker-sentry/blob/3115587c614e64c66419a26b4f7be6ac067e3a79/Dockerfile)

# 什么是Sentry?

Sentry 是一个实时事件日志及聚集平台.它特别适合监视错误,为事后调查分析,萃取所有需要的信息,而没有标准的用户反馈迭代的麻烦.

> [github.com/getsentry/sentry](https://github.com/getsentry/sentry)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/sentry/logo.png)

# 怎么使用镜像

## 建立完整的sentry 实例

1.	启动redis容器

	```console
	$ docker run -d --name some-redis redis
	```

2.	启动数据库容器:

	-	Postgres (上游推荐):

		```console
		$ docker run -d --name some-postgres -e POSTGRES_PASSWORD=secret -e POSTGRES_USER=sentry postgres
		```

	-	MySQL (后续步骤假设使用 PostgreSQL, 可以使用 `--link some-mysql:mysql` 替换 `--link some-postgres:postres`):

		```console
		$ docker run -d --name some-mysql -e MYSQL_ROOT_PASSWORD=secret -e MYSQL_DATABASE=sentry mysql
		```

3.	启动sentry 服务

	```console
	$ docker run -d --name some-sentry --link some-redis:redis --link some-postgres:postgres sentry
	```

4.	如果是一个新的数据库, 需要运行 `sentry upgrade`

	```console
	$ docker run -it --rm --link some-postgres:postgres --link some-redis:redis sentry sentry upgrade
	```

	**注意: `-it` 参数是重要的,因为初始升级会提示创建初始用户,没有它会失败 **

5.	默认配置需要使用一个celery beat, 多个celery worker,依据你的需要启动启动多个worker(每一个使用不同的名字)

	```console
	$ docker run -d --name sentry-celery-beat --link some-postgres:postgres --link some-redis:redis sentry sentry celery beat
	$ docker run -d --name sentry-celery1 --link some-postgres:postgres --link some-redis:redis sentry sentry celery worker
	```

### 端口映射

如果你想没有容器ip地址情况下访问容器实例,可以使用标准的端口映射.给`docker run` 命令添加`-p 8080:9000`, 然后浏览器里打开`http://localhost:8080`或者`http://host-ip:8080`访问.

## 配置初始用户

如果在执行`sentry upgrade`时没有创建超级用户, 使用下述创建一个:

```console
$ docker run -it --rm --link some-postgres:postgres sentry sentry createsuperuser
```

