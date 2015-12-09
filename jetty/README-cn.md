# 简介
Jetty 是一个Java实现的开源的Java HTTP服务器和servlet容器，可以为基于Java的web容器如JSP、servlet提供运行环境。Jetty当前还经常在大型的软件系统中用于机器之间的通讯。Jetty是Eclipse基金会的一个免费开源项目。Jetty支持最新的Java Servlet API，此外还支持SPDY协议和WebSocket协议。

> [wikipedia.org/wiki/Jetty_(web_server)](https://en.wikipedia.org/wiki/Jetty_%28web_server%29)

![logo](https://raw.githubusercontent.com/docker-library/docs/master/jetty/logo.png)Logo &copy; Eclipse Foundation

# 如何使用

直接后台启动一个jetty实例：

```console
$ docker run -d jetty
```

启动后通过浏览器访问 `http://容器IP地址:8080` 或 `https://容器IP地址:8443/`, 
如果要使用Jetty对外部网络提供服务，使用docker的端口映射参数:

```console
$ docker run -d -p 80:8080 -p 443:8443 jetty
```

这样直接将jetty容器的8080端口对外映射为80端口，容器的8443端口对外映射为443端口，
可以直接使用浏览器访问 `http://主机IP地址` 或者 `https://主机IP地址`

## 环境变量

容器默认的环境变量如下:

	JETTY_HOME    =  /usr/local/jetty
	JETTY_BASE    =  /var/lib/jetty
	TMPDIR        =  /tmp/jetty

## 部署应用

通常情况下，请将你的web应用[部署](https://www.eclipse.org/jetty/documentation/current/quickstart-deploying-webapps.html)在`/var/lib/jetty/webapps`，如果要部署在`/`目录下,则必须使用文件名 `ROOT.war`，目录名 `ROOT`，或者配置文件`ROOT.xml`.

## 配置

使用命令行选项`--list-config`打印Jetty服务器当前使用的配置参数：

```console
$ docker run -d jetty --list-config
```

使用命令行参数使用其他模块或指定参数，如:

```console
$ docker run -d jetty --modules=jmx jetty.threadPool.maxThreads=500
```

想要更新配置，还可以使用`Dockerfile`重新衍生一个新的镜像，比如修改RUN指令来激活额外的模块:

```Dockerfile
FROM jetty

RUN java -jar "$JETTY_HOME/start.jar" --add-to-startd=jmx,stats
```

如果想要调整Jetty模块的配置，可以在`Dockerfile`中编辑对应的`/var/lib/jetty/start.d/*.mod`的参数，如果想要停用某个模块，直接删除`/var/lib/jetty/start.d/*.mod`对应的模块文件即可

## 只读运行容器

运行一个完全只读的`jetty`容器，只需要从容器外部以volumn形式挂载`/tmp/jetty`和`/run/jetty`目录即可：

```console
$ docker run -d --read-only -v /tmp/jetty -v /run/jetty jetty
```

除此以外，想要运行你的Web应用，你还需要从容器外部挂载你的web应用目录`-v /path/to/my/webapps:/var/lib/jetty/webapps`，或者在你制作衍生镜像的时候，将你的web应用打包到镜像的`/var/lib/jetty/webapps`

## HTTP/2 支持

从9.3版本开始，jetty内置支持了HTTP/2。但由于潜在的授权问题，默认并未激活该功能模块。如果想要在个人使用的衍生镜像中使用HTTP2的功能支持，你可以在衍生的`Dockerfile`中使用`RUN`来开启`http2`的支持，如下:

```Dockerfile
FROM jetty

RUN java -jar \$JETTY_HOME/start.jar --add-to-startd=http2 --approve-all-licenses
```

这样将会添加`http2.ini`文件到`$JETTY_BASE/start.d`目录并自动下载所需的ALPN库到`$JETTY_BASE/lib/alpn`，这样就可以正常使用HTTP2了。HTTP/2连接应该和HTTPS一样走容器的8443端口。如果你想通过`$JETTY_BASE/start.ini`来激活`http2`模块，替换上面RUN指令中的`--add-to-startd`为`--add-to-start`

一旦OpenJDK9内置支持了ALPN，此镜像将更新为默认开启HTTP2的支持

# 安全

默认情况下，镜像启动使用`root`账号初始化，之后使用Jetty的`setuid`模块降低为普通用户`jetty`权限来运行。`JETTY_BASE`目录`/var/lib/jetty`的属主为`jetty:jetty` (uid 999, gid 999).
如果你想镜像从启动开始就使用 `jetty`账号而不是`root`，可以使用参数：`-u jetty`:

```console
$ docker run -d -u jetty jetty
```

# 授权

查看[授权](http://eclipse.org/jetty/licenses.php)
