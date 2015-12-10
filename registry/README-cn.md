# 支持的镜像tag 和 `Dockerfile` 链接

-	[`latest`, `0.9.1` (*Dockerfile*)](https://github.com/docker/docker-registry/blob/0.9.1/Dockerfile)
-	[`0.8.1` (*Dockerfile*)](https://github.com/docker/docker-registry/blob/0.8.1/Dockerfile)
-	[`2`, `2.2`, `2.2.0` (*Dockerfile*)](https://github.com/docker/distribution-library-image/blob/af287a2a6379ac099edc9c76672abe5cbe2f9469/Dockerfile)


# docker 镜像仓库

标记2以上参考 [new registry](https://github.com/docker/distribution).

更老的参考 [deprecated registry](https://github.com/docker/docker-registry).

## 运行仓库

-	根据[下书指令](http://docs.docker.io/installation/#installation)安装docker

### 快速启动docker registry 容器

-	run the registry: `docker run -p 5000:5000 -v <HOST_DIR>:/tmp/registry-dev registry`
-	Modify your docker startup line/script: add "-H tcp://127.0.0.1:2375 -H unix:///var/run/docker.sock --insecure-registry <REGISTRY_HOSTNAME>:5000"

### 推荐方式启动docker regsitry 容器

```console
$ docker run \
         -e SETTINGS_FLAVOR=s3 \
         -e AWS_BUCKET=acme-docker \
         -e STORAGE_PATH=/registry \
         -e AWS_KEY=AKIAHSHB43HS3J92MXZ \
         -e AWS_SECRET=xdDowwlK7TJajV1Y7EoOZrmuPEJlHYcNP2k4j49T \
         -e SEARCH_BACKEND=sqlalchemy \
         -p 5000:5000 \
         registry
```

注意: 容器会占用5000 端口, 如果已经被占用, 使用`docker ps` 查出哪个容器占用.

