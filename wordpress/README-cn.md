# 支持的镜像tag 和 `Dockerfile` 链接

-	[`4.3.1-apache`, `4.3.1`, `4.3-apache`, `4.3`, `4-apache`, `apache`, `4`, `latest` (*apache/Dockerfile*)](https://github.com/docker-library/wordpress/blob/ef064e49ebedfa12cf27e94c58b6ec103ae9b816/apache/Dockerfile)
-	[`4.3.1-fpm`, `4.3-fpm`, `4-fpm`, `fpm` (*fpm/Dockerfile*)](https://github.com/docker-library/wordpress/blob/ef064e49ebedfa12cf27e94c58b6ec103ae9b816/fpm/Dockerfile)

# 什么是WordPress?

WordPress 是一个自由开源的博客工具,内容管理系统(CMS),基于php和mysql开发,运行在web托管服务上,特性包括插件架构和一个模板系统.2013年八月时,前一千万站点中,有超过22%在使用WordPress.WordPress是web上最流行的博客系统,超过六千万站点使用.使用的语言中最流行的是英语,西班牙语,印尼语.

> [wikipedia.org/wiki/WordPress](https://en.wikipedia.org/wiki/WordPress)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/wordpress/logo.png)

# 怎么使用镜像

```console
$ docker run --name some-wordpress --link some-mysql:mysql -d wordpress
```

配置你的WordPress实例,会用到以下环境变量:

-	`-e WORDPRESS_DB_HOST=...` (被链接的mysql容器的默认ip和端口)
-	`-e WORDPRESS_DB_USER=...` (数据库默认用户 "root")
-	`-e WORDPRESS_DB_PASSWORD=...` (mysql容器默认数据库密码)
-	`-e WORDPRESS_DB_NAME=...` (mysql容器默认数据库名字 "wordpress")
-	`-e WORDPRESS_TABLE_PREFIX=...` (默认是 "",当你想覆盖wp-config.php中的默认表前缀时,设置它)
-	`-e WORDPRESS_AUTH_KEY=...`, `-e WORDPRESS_SECURE_AUTH_KEY=...`, `-e WORDPRESS_LOGGED_IN_KEY=...`, `-e WORDPRESS_NONCE_KEY=...`, `-e WORDPRESS_AUTH_SALT=...`, `-e WORDPRESS_SECURE_AUTH_SALT=...`, `-e WORDPRESS_LOGGED_IN_SALT=...`, `-e WORDPRESS_NONCE_SALT=...` (默认是唯一随机的SHA1值)

如果`WORDPRESS_DB_NAME`指定的数据库在给定的mysql server上不存在,在启动`wordpress`容器时会自动创建,给`WORDPRESS_DB_USER`指定的用户提供必要的权限来创建数据库.

不知道容器ip,访问容器,可以使用标准的端口映射:

```console
$ docker run --name some-wordpress --link some-mysql:mysql -p 8080:80 -d wordpress
```

然后通过在浏览器里打开`http://localhost:8080`或者`http://host-ip:8080`访问.

如果你想使用外部数据库替代链接的mysql容器,使用`WORDPRESS_DB_HOST`指定数据库的主机名和端口,`WORDPRESS_DB_PASSWORD`指定数据库密码, `WORDPRESS_DB_USER`指定用户(如果用户不是root):

```console
$ docker run --name some-wordpress -e WORDPRESS_DB_HOST=10.1.2.3:3306 \
    -e WORDPRESS_DB_USER=... -e WORDPRESS_DB_PASSWORD=... -d wordpress
```

## 使用 [`docker-compose`](https://github.com/docker/compose)

Example `docker-compose.yml` for `wordpress`:

```yaml
wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 8080:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: example
```

运行 `docker-compose up`, 待其完全初始化,打开浏览器访问`http://localhost:8080`或`http://host-ip:8080`.

## 添加额外的库和拓展

本镜像并没有提供任何额外的php拓展或者其他库,即使是流行插件所需要的.可能存在数目无限的插件,它们可能会要求任何php拓展支持.引入每一个存在的php拓展,会极具增加镜像的大小.

如果你需要额外的php拓展,那就已此镜像为基础,制作自己的镜像.The [documentation of the `php` image](https://github.com/docker-library/docs/blob/master/php/README.md#how-to-install-more-php-extensions)解释了怎么编译拓展.另外,[`wordpress` Dockerfile](https://github.com/docker-library/wordpress/blob/618490d4bdff6c5774b84b717979bfe3d6ba8ad1/apache/Dockerfile#L5-L9)有一个例子.

下面的docker hub 特性,有助于帮助保持你的依赖镜像最新:

-	[Automated Builds](https://docs.docker.com/docker-hub/builds/) let Docker Hub automatically build your Dockerfile each time you push changes to it.
-	[Repository Links](https://docs.docker.com/docker-hub/builds/#repository-links) can ensure that your image is also rebuilt any time `wordpress` is updated.

