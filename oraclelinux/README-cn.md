# 支持的镜像tag 和 `Dockerfile` 链接

-	[`latest`, `7`, `7.1` (*OracleLinux/7.1/Dockerfile*)](https://github.com/oracle/docker/blob/cd11b463dcf97128b5472d33d6c1fdd6a8099e23/OracleLinux/7.1/Dockerfile)
-	[`7.0` (*OracleLinux/7.0/Dockerfile*)](https://github.com/oracle/docker/blob/cd11b463dcf97128b5472d33d6c1fdd6a8099e23/OracleLinux/7.0/Dockerfile)
-	[`6`, `6.7` (*OracleLinux/6.7/Dockerfile*)](https://github.com/oracle/docker/blob/cd11b463dcf97128b5472d33d6c1fdd6a8099e23/OracleLinux/6.7/Dockerfile)
-	[`6.6` (*OracleLinux/6.6/Dockerfile*)](https://github.com/oracle/docker/blob/cd11b463dcf97128b5472d33d6c1fdd6a8099e23/OracleLinux/6.6/Dockerfile)
-	[`5`, `5.11` (*OracleLinux/5.11/Dockerfile*)](https://github.com/oracle/docker/blob/cd11b463dcf97128b5472d33d6c1fdd6a8099e23/OracleLinux/5.11/Dockerfile)

关于此镜像的更多信息和历史, 请看 [the relevant manifest file (`library/oraclelinux`)](https://github.com/docker-library/official-images/blob/master/library/oraclelinux). This image is updated via pull requests to [the `docker-library/official-images` GitHub repo](https://github.com/docker-library/official-images).

For detailed information about the virtual/transfer sizes and individual layers of each of the above supported tags, please see [the `oraclelinux/tag-details.md` file](https://github.com/docker-library/docs/blob/master/oraclelinux/tag-details.md) in [the `docker-library/docs` GitHub repo](https://github.com/docker-library/docs).

# Oracle Linux

![logo](https://raw.githubusercontent.com/docker-library/docs/master/oraclelinux/logo.png)

Oracle Linux 是一款使用GPLv2 发布开源操作系统. 适合一般任务或者oracle相关任务, 通过每天超过128,000小时真实环境的工作量的严格测试, 使用了独特的创新技术,包括ksplice (零宕机时间内核更新), Dtrace(实时追踪诊断), 强大的btrfs 文件系统等

## 怎么使用这些镜像

这些镜像意图被用于应用Dockerfile ` **FROM** ` 指令. 比如使用oracle linux 版本6 作为基础镜像, `FROM oraclelinux:6`.

## 官方资源

-	[Learn more about Oracle Linux](http://oracle.com/linux)
-	[Unbreakable Linux Network](https://linux.oracle.com)
-	[Oracle Public Yum](http://public-yum.oracle.com)

## 社交媒体资源

-	[Twitter](https://twitter.com/ORCL_Linux)
-	[Facebook](https://www.facebook.com/OracleLinux)
-	[YouTube](https://www.youtube.com/user/OracleLinuxChannel)
-	[Blog](http://blogs.oracle.com/linux)

# 许可证

使用本镜像包含的软件的许可, 请查看 [Oracle Linux End-User License Agreement](https://oss.oracle.com/ol6/EULA)

# 支持的docker 版本

此镜像支持docker 官方版本 1.9.1.

更老的docker 版本往前到1.6
