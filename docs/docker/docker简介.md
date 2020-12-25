## docker 定义

> 根据官方定义，docker 是以 docker 容器为资源分割和调度的基本单位，封装整个软件运行时环境，为开发者和系统管理员是设计的，用于构建、发布和运行分布式应用的平台，它是一个跨平台，可移植并且简单易用的容器解决方案。docker 的源代码托管在 Github 上，基于 Go 语言开发并遵从 Apache 2.0 协议。Docker 可在容器内部快速自动化部署应用，并通过操作系统内核技术（namespaces,cgroup 等）为容器提供资源隔离与安全保障

Docker 通常用于如下场景：

- web 应用的自动化打包和发布；
- 自动化测试和持续集成、发布；
- 在服务型环境中部署和调整数据库或其他的后台应用；
- 从头编译或者扩展现有的云平台来搭建自己的 PaaS 环境。

_Q&A_

> 各种虚拟机技术开启了云计算时代；而 Docker，作为下一代虚拟化技术，正在改变我们开发、测试、部署应用的方式。那虚拟机与 Docker 究竟有何不同呢?

使用虚拟机和 Docker 运行多个相互隔离的应用时，如下图所示:

![image](https://cdn.yuanmedia.top/bolg/0062z5a3gy1fpshy1zq0sj30fe07nglt.jpg)

vm 与 docker 框架，直观上来讲 vm 多了一层 guest OS，同时 Hypervisor 会对硬件资源进行虚拟化，docker 直接使用硬件资源，所以资源利用率相对 docker 低非常多

做个数据对比

openstack 能够以 10 台/min 的速度创建虚拟机，而 docker 几秒钟之内启动几十甚至几百个容器

## 基本名词

**镜像**

Docker 镜像是一个只读的 Docker 容器模板，含有启动 Docker 容器所需的文件系统结构以及内容，因此是启动一个 Docker 容器的基础。Docker 镜像的文件内容以及一些运行 Docker 容器的配置文件组成了 Docker 容器的静态文件系统运行环境

**容器**

Docker 容器就是 Docker 镜像的运行状态

**Dockerfile**

通过 Dockerfile 可以生成对应的镜像
dockerFile 分为四部分组成：基础镜像信、维护者信息、镜像操作指令和容器启动时执行指令。例如：python app

```
FROM python:3.4
RUN apt-get update
RUN apt-get install apt-transport-https -y
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
RUN curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
RUN apt-get update
RUN ACCEPT_EULA=Y apt-get install msodbcsql -y
RUN ACCEPT_EULA=Y apt-get install mssql-tools  -y
RUN export PATH="$PATH:/opt/mssql-tools/bin"
RUN export PATH="$PATH:/opt/mssql-tools/bin"
RUN apt-get install unixodbc-dev -y
RUN export DEBIAN_FRONTEND=noninteractive
RUN apt-get install locales
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen
RUN locale-gen
ADD MscnCms_backend  /usr/MscnCms_backend
ADD ./marketsmith.cfg /etc/
WORKDIR /usr/MscnCms_backend
RUN pip install -r requirements.txt
```

nginx

```
FROM nginx
ADD cms/ /var/www/cms
ADD mscncms.conf /etc/nginx/conf.d/
RUN  export LC_ALL=C.UTF-8
```

**docker-compose**

容器编排和部署的工具

如下图所示

```
version: '2'
services:
  backend:
    image: admin_backend
    ports:
      - "8000:8000"
    container_name: backend
    extra_hosts:
      - "WONALIMSDBPROD1:192.168.178.21"
      - "WONALIDBSTAGE:192.168.178.101"
    command: python manage.py runserver 0.0.0.0:8000
  web:
    restart: always
    image: admin_nginx
    ports:
       - "80:8088"
    depends_on:
       - backend
    links:
       - backend
```

## 容器生态系统

![image](https://cdn.yuanmedia.top/bolg/0062z5a3gy1fpshobbv3zj30gu0a1wfq.jpg)

## 安装

#### Docker CE

进入[官网](https://docs.docker.com/install)，选择相应平台，下载并运行安装文件即可

#### docker-compose

```
pip install docker-compose
```

## docker 的优缺点

**优点**

我觉得 Docker 做得最好的事情就是让开发者、用户和大家能够在任何地方非常容易地运行一个应用程序。它几乎就是开发者的圣杯，因为你既可以在你的桌面上运行一个应用程序，而且不需要任何修改，你就可以在服务器上毫无差异地运行这个相同的应用程序。这在之前是从来没有出现过的。基于 Docker 的特性，在微服务方面有着巨大的优势

**不足**

Docker 本身是简单的，而实现 Docker 是复杂的，它需要很多技术的支撑，比如说，容器管理、编排、应用打包、容器间的网络、数据快照等等。现在使用 Docker，你必须习惯使用命令行，还没有可视化界面，都是命令行指令。需要一个 GUI 可以更加简单一点
