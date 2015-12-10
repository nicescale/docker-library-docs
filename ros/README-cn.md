# 支持的镜像tag 和 `Dockerfile` 链接

-	[`indigo-ros-core` (*ros/indigo/indigo-ros-core/Dockerfile*)](https://github.com/osrf/docker_images/blob/8a11109079636bcd3bdf341993d39e2b7d503c6c/ros/indigo/indigo-ros-core/Dockerfile)
-	[`indigo-ros-base`, `indigo`, `latest` (*ros/indigo/indigo-ros-base/Dockerfile*)](https://github.com/osrf/docker_images/blob/8a11109079636bcd3bdf341993d39e2b7d503c6c/ros/indigo/indigo-ros-base/Dockerfile)
-	[`indigo-robot` (*ros/indigo/indigo-robot/Dockerfile*)](https://github.com/osrf/docker_images/blob/8a11109079636bcd3bdf341993d39e2b7d503c6c/ros/indigo/indigo-robot/Dockerfile)
-	[`indigo-perception` (*ros/indigo/indigo-perception/Dockerfile*)](https://github.com/osrf/docker_images/blob/8a11109079636bcd3bdf341993d39e2b7d503c6c/ros/indigo/indigo-perception/Dockerfile)
-	[`jade-ros-core` (*ros/jade/jade-ros-core/Dockerfile*)](https://github.com/osrf/docker_images/blob/d579b9325fd8546a29dfc064661b005cfbc9cf8b/ros/jade/jade-ros-core/Dockerfile)
-	[`jade-ros-base`, `jade` (*ros/jade/jade-ros-base/Dockerfile*)](https://github.com/osrf/docker_images/blob/d579b9325fd8546a29dfc064661b005cfbc9cf8b/ros/jade/jade-ros-base/Dockerfile)
-	[`jade-robot` (*ros/jade/jade-robot/Dockerfile*)](https://github.com/osrf/docker_images/blob/d579b9325fd8546a29dfc064661b005cfbc9cf8b/ros/jade/jade-robot/Dockerfile)
-	[`jade-perception` (*ros/jade/jade-perception/Dockerfile*)](https://github.com/osrf/docker_images/blob/d579b9325fd8546a29dfc064661b005cfbc9cf8b/ros/jade/jade-perception/Dockerfile)

# 什么是 [ROS](http://www.ros.org/)?

机器人操作系统(ROS)是软件库和工具的集合, 用以辅助你构建机器人相关的应用.从驱动到最先进算法,强大的开发工具,ROS 能满足你的下一代机器人项目的所有需求.同时, ROS是完全开源的.

> [wikipedia.org/wiki/Robot_Operating_System](https://en.wikipedia.org/wiki/Robot_Operating_System)

[![logo](https://raw.githubusercontent.com/docker-library/docs/master/ros/logo.png)](http://www.ros.org/)

# 怎么使用镜像

## 在你的ros应用目录里创建 `Dockerfile` 

```dockerfile
FROM ros:indigo
# place here your application's setup specifics
CMD [ "roslaunch", "my-ros-app my-ros-app.launch" ]
```

构建运行你的docker 镜像:

```console
$ docker build -t my-ros-app .
$ docker run -it --rm --name my-running-app my-ros-app
```

## 部署案例

这个ROS docker 镜像旨在为构建部署分发的机器人应用提供简单一致的平台.基于[官方ubuntu镜像](https://registry.hub.docker.com/_/ubuntu/)和ROS 官方debian 包,包含了最近支持的软件释放.本镜像为学术界和工业界的机器人学者开发,重用,分发, 自治动作,任务规划,控制动力学,定位,地图,群体行为等类型的应用提供了便利, 同样提供了常见的功能模块的集成.

## 部署建议

可用的镜像标记:  
- `ros-core`: ROS 核心安装
- `ros-base`: 基本工具和库 (also tagged with distro name with LTS version as `latest`)  
- `robot`: 机器人基本安装 
- `perception`: 感知任务的基本安装

### 卷

ROS 使用`~/.ros/` 目录存储日志和调试信息.如果你想保存下这些信息,可以把`~/.ros/`以主机上外部卷挂载到容器内部,或者用户定义的子镜像可以指定卷有docker 引擎管理.默认会以`root` 用户运行容器,所以`/root/.ros/`会是这些文件的完整路径.


希望把数据存在主机家目录下某用户名下,比如 `ubuntu`, 启动容器时, -v 多添加一参数:

```console
$ docker run -v "/home/ubuntu/.ros/:/root/.ros/" ros
```

### 设备

一些应用可能要求主机上设备访问,如从摄像头获取图片,控制HID设备的输入,使用gpu的硬件加速. 可通过[`--device`](https://docs.docker.com/reference/run/)挂载主机上设备进入容器内部,提供硬件访问.

### 网络 

ROS 运行时结构图是由使用ROS通讯基础设施的点对点通信网络进程(可能分散在不同的主机上)松散耦合而成.ROS实现了几种不同通讯风格,包括服务上使用的同步rpc ,使用主题发布的异步数据流, 使用Parameter server存储数据.为了遵循[每容器一进程](https://docs.docker.com/articles/dockerfile_best-practices/)的最佳实践,docker网络可以用于把几个运行的ROS进程连接一起.关于[ROS网络设置](http://wiki.ros.org/ROS/NetworkSetup)的更多细节,查看下面部署例子.


## 部署例子

**注意:** 这个要求使用docker 1.9 版本的网络功能.如果我们想要所有ROS 节点能相互之间沟通顺畅,可以创建一个虚拟网络,启动一个新容器运行`roscore` 作为网络中的 `master`服务,然后在此网络启动消息发布者和订阅者服务.

### 构建镜像

> 使用这个`Dockerfile` 构建包含 ROS 指南的 ROS 镜像:

```dockerfile
FROM ros:indigo-ros-base
# install ros tutorials packages
RUN apt-get update && apt-get install -y
    ros-indigo-ros-tutorials \
    ros-indigo-common-tutorials \
    && rm -rf /var/lib/apt/lists/
```

> 然后在同一个目录内,构建镜像:

```console
$ docker build --tag ros:ros-tutorials .
```

#### 创建网络

> 为了创建新的网络 `foo`, 我们使用网络创建命令:

	docker network create foo

> 现在我们有了一个网络,可以创建服务. 服务通告了它们在网络中的位置,使得解析提供服务的容器的位置/地址变得容易.我们将会使用此特性,确保ROS 节点找到并连接上master. 

#### 运行服务

> 为了创建ROS master容器, 通告它的服务:

```console
$ docker run -it --rm\
    --publish-service=master.foo \
    --name master \
    ros:ros-tutorials \
    roscore
```

> 现在,你可以看见master 正在运行,准备管理我们的其他ROS 节点. 为了添加我们的`talker` 节点, 需要使用相关的环境变量指向master 服务:

```console
$ docker run -it --rm\
    --publish-service=talker.foo \
    --env ROS_HOSTNAME=talker \
    --env ROS_MASTER_URI=http://master:11311 \
    --name talker \
    ros:ros-tutorials \
    rosrun roscpp_tutorials talker
```

> 在一个终端, 运行 `listener` 节点:

```console
$ docker run -it --rm\
    --publish-service=listener.foo \
    --env ROS_HOSTNAME=listener \
    --env ROS_MASTER_URI=http://master:11311 \
    --name listener \
    ros:ros-tutorials \
    rosrun roscpp_tutorials listener
```

>  你应该看见 `listener` 在回应 `talker` 广播的每一条消息. 你可以列出一些容器,如下呈现:

```console
$ docker service ls
SERVICE ID          NAME                NETWORK             CONTAINER
67ce73355e67        listener            foo                 a62019123321
917ee622d295        master              foo                 f6ab9155fdbe
7f5a4748fb8d        talker              foo                 e0da2ee7570a
```

> And for the services:

```console
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED              STATUS              PORTS               NAMES
a62019123321        ros:ros-tutorials   "/ros_entrypoint.sh    About a minute ago   Up About a minute   11311/tcp           listener
e0da2ee7570a        ros:ros-tutorials   "/ros_entrypoint.sh    About a minute ago   Up About a minute   11311/tcp           talker
f6ab9155fdbe        ros:ros-tutorials   "/ros_entrypoint.sh    About a minute ago   Up About a minute   11311/tcp           master
```

#### 自省

> 现在,我们看到两个节点在通讯, 进入其中一个容器,检查一下主题内容

```console
$ docker exec -it master bash
$ source /ros_entrypoint.sh
```

> 如果我们使用 `rostopic` 查看发布的消息主题, 会看到一些类似如下的信息:

```console
$ rostopic list
/chatter
/rosout
/rosout_agg
```

#### 销毁

> 为了销毁我们创建的,仅需停止容器和服务. 可以使用`Ctrl^C` 停掉删除容器或者使用stop 命令:
```console
$ docker stop master talker listener
$ docker rm master talker listener
```

# 更多资源

[ROS.org](http://www.ros.org/): Main ROS website  
[Wiki](http://wiki.ros.org/): Find tutorials and learn more  
[ROS Answers](http://answers.ros.org/questions/): Ask questions. Get answers  
[Blog](http://www.ros.org/news/): Stay up-to-date  
[OSRF](http://www.osrfoundation.org/): Open Source Robotics Foundation
