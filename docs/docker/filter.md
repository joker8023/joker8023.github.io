## Swarm 过滤器

Docker swarm 定时任务可以与多个过滤器一起使用。

下面是当前为一系列节点调度容器时可用的过滤器。

- Constraint
- Affinity
- Port
- Dependency
- Health

使用 swarm manage 时以--filter 标志就可选择所需的过滤器。

## Constraint（约束）

过滤器 Constraint 是和特定节点相关的键值对。可以视作节点标志。

创建容器时，用户选择一系列被分配有用一组或多组键值对指定的定时任务的节点。

这种有几种特殊用用途如：选择特定主机属性（如 storage=ssd

为了在特定硬件下调度容器。）根据物理位置标记节点 (region=us-east, 强制容器在指定位置运行).集群逻辑分区(environment=production, 将集群分成不同属性的子集群)

用一系列键值对标记节点，必须在 docker 启动时传递--label、

如 以 storage=ssd 标签启动 node-1

    $ docker -d --label storage=ssd
    $ swarm join --addr=192.168.0.42:2375 token://XXXXXXXXXXXXXXXXXX

再次以 storage=disk 标签启动 node-2

    $ docker -d --label storage=disk
    $ swarm join --addr=192.168.0.43:2375 token://XXXXXXXXXXXXXXXXXX

一旦节点在集群注册，master 分别获取各自标签。当调度新容器时会使用这些参数。

让我们启动一个 mysql 服务器并以闪存驱动选择节点确保节点具有良好的 IO 性能：

    $ docker run -d -P -e constraint:storage==ssd --name db mysql
    f8b693db9cd6

    $ docker ps
    CONTAINER ID        IMAGE               COMMAND             CREATED                  STATUS              PORTS                           NODE        NAMES
    f8b693db9cd6        mysql:latest        "mysqld"            Less than a second ago   running             192.168.0.42:49178->3306/tcp    node-1      db

这种情况下，master 选择所有带 storage=ssd 约束的节点，如之前所说，再在其之上应用资源管理。这个例子中，node-1 被选择，因为这事唯一一个安装有闪存的主机。

现在再尝在集群中试运行 nginx frontend。然而不想使用闪存驱动，因为主要是用于向硬盘写日志。

    $ docker run -d -P -e constraint:storage==disk --name frontend nginx
    963841b138d8

    $ docker ps
    CONTAINER ID        IMAGE               COMMAND             CREATED                  STATUS              PORTS                           NODE        NAMES
    963841b138d8        nginx:latest        "nginx"             Less than a second ago   running             192.168.0.43:49177->80/tcp      node-2      frontend
    f8b693db9cd6        mysql:latest        "mysqld"            Up About a minute        running             192.168.0.42:49178->3306/tcp    node-1      db

调度器选择了 node-2，因为其以 storage=disk 标签启动。

## 标准的 Constraint

补充一下，如果节点启动时没有指定 constraint 则调度器会使用标准的 constraint。这些标签来自于 docker info，目前有：

- storagedriver
- executiondriver
- kernelversion
- operatingsystem

## Affinity（姻亲） 过滤器

### 容器

可以调度两个容器，让容器#2 紧邻容器#1

    $ docker run -d -p 80:80 --name front nginx 87c4376856a8

     $ docker ps

    CONTAINER ID        IMAGE               COMMAND             CREATED                  STATUS              PORTS                           NODE        NAMES

    87c4376856a8        nginx:latest        "nginx"             Less than a second ago   running             192.168.0.42:80->80/tcp         node-1      front

使用-e affinity:container==front 会调度一个紧邻容器 front 的容器。也可以使用 ID 代替名字：-e affinity:container==87c4376856a8

    $ docker run -d --name logger -e affinity:container==front logger
     87c4376856a8

    $ docker ps
    CONTAINER ID        IMAGE               COMMAND             CREATED                  STATUS              PORTS                           NODE        NAMES
    87c4376856a8        nginx:latest        "nginx"             Less than a second ago   running             192.168.0.42:80->80/tcp         node-1      front
    963841b138d8        logger:latest       "logger"            Less than a second ago   running                                             node-1      logger

由于和 front 的姻亲，logger 容器在 node-1 停止 。

### Images

可以仅仅在已经拉取某镜像的节点上调度一个容器。

    $ docker -H node-1:2375 pull redis

    $ docker -H node-2:2375 pull mysql

    $ docker -H node-3:2375 pull redis

这里仅仅 node-1 和 node-3 有 redis 镜像。 使用 -e affinity:image=redis 可以只调度这两个节点的容器。也可用 ID 而非名字：

    $ docker run -d --name redis1 -e affinity:image==redis redis

    $ docker run -d --name redis2 -e affinity:image==redis redis

    $ docker run -d --name redis3 -e affinity:image==redis redis

    $ docker run -d --name redis4 -e affinity:image==redis redis

    $ docker run -d --name redis5 -e affinity:image==redis redis

    $ docker run -d --name redis6 -e affinity:image==redis redis

    $ docker run -d --name redis7 -e affinity:image==redis redis

    $ docker run -d --name redis8 -e affinity:image==redis redis

    $ docker ps

    CONTAINER ID        IMAGE               COMMAND             CREATED                  STATUS              PORTS                           NODE        NAMES

    87c4376856a8        redis:latest        "redis"             Less than a second ago   running                                             node-1      redis11212386856a8        redis:latest        "redis"             Less than a second ago   running                                             node-1      redis287c4376639a8        redis:latest        "redis"             Less than a second ago   running                                             node-3      redis31234376856a8        redis:latest        "redis"             Less than a second ago   running                                             node-1      redis486c2136253a8        redis:latest        "redis"             Less than a second ago   running                                             node-3      redis587c3236856a8        redis:latest        "redis"             Less than a second ago   running                                             node-3      redis687c4376856a8        redis:latest        "redis"             Less than a second ago   running                                             node-3      redis7963841b138d8        redis:latest        "redis"             Less than a second ago   running                                             node-1      redis8

如上所见，仅仅是 redis 镜像被拉取的节点上的容器被调度。

### Expression Syntax 表达式语法

姻亲或者约束表达式有键值组成。键和 alpha-numeric 模式一致，开头必须是字母或者下划线。值必须是下列中的一个：一个数字或者字母组成的字符串，点，连接符和下划线；

一个 A globbing 模式，如.abc*. *；形如 /regexp/的正则表达式.支持 Go 语音的正则表达式 。

目前 swarm 支持 affinity/constraint 操作如== 和 !=

如 constraint:node==node1 匹配节点 node1. constraint:node!=node1 除 node1 外的所有节点。 constraint:region!=us\* 将匹配所有不在以 us 的域（regions）中的节点。 constraint:node==/node[12]/ 匹配 节点 node1 and node2. constraint:node==/node\d/将匹配所有 node 加上一个数字组成发节点 constraint:node!=/node-[01]/ 匹配除 node-0 and node-1 节点的所有节点 constraint:node!=/foo\[bar\]/ 除 foo[bar]节点的所有节点。 可能会使用转义字符。constraint:node==/(?i)node1/ 无大小写敏感的匹配 node1 . 所有 'NoDe1' 或'NODE1' 也会匹配。

### 软姻亲/软约束（Soft Affinities/Constraints）

默认情况下，姻亲和约束是硬性的。如果一个姻亲或约束不匹配，容器就不会调度。软姻亲和软约束会先匹配规则，如不符合则按策略调度。

软姻亲/软约束以~表示，如下例。

    $ docker run -d --name redis1 -e affinity:image==~redis redis

如果集群中没有节点有 redis 镜像，调度器会放弃姻亲而使用策略调度。

    $ docker run -d --name redis2 -e constraint:region==~us* redis

如果没有节点属 us 域, 调度器会放弃约束而使用策略调度。

    $ docker run -d --name redis5 -e affinity:container!=~redis* redis

姻亲过滤器将会用于调度一个新的名字匹配 redis*的 redis 容器。如果没有满 redis*的容器。调度器会放弃姻亲规则而根据策略调度。

## 端口过滤器（Port Filter）

这种过滤器将端口视作独特资源。

    $ docker run -d -p 80:80 nginx87c4376856a8

    $ docker ps

    CONTAINER ID    IMAGE               COMMAND         PORTS                       NODE        NAMES

    87c4376856a8    nginx:latest        "nginx"         192.168.0.42:80->80/tcp     node-1      prickly_engelbart

Docker 集群选择一个 80 端口可用的节点，在其之上调度容器 ，例中选择了 node-1.

尝试以 80 端口运行另一个容器会使集群选择另一个节点，因为 node-1 该端口已经占用。

    $ docker run -d -p 80:80 nginx963841b138d8

     $ docker ps

    CONTAINER ID        IMAGE          COMMAND        PORTS                           NODE        NAMES

    963841b138d8        nginx:latest   "nginx"        192.168.0.43:80->80/tcp         node-2      dreamy_turing87c4376856a8        nginx:latest   "nginx"        192.168.0.42:80->80/tcp         node-1      prickly_engelbart

同样, 再次使用此命令用选择 node-3, 因为 node-1 和 node-2 的 80 端口都被占用了。

    $ docker run -d -p 80:80 nginx963841b138d8

    $ docker ps

    CONTAINER ID   IMAGE               COMMAND        PORTS                           NODE        NAMES

    f8b693db9cd6   nginx:latest        "nginx"        192.168.0.44:80->80/tcp         node-3      stoic_albattani963841b138d8   nginx:latest        "nginx"        192.168.0.43:80->80/tcp         node-2      dreamy_turing87c4376856a8   nginx:latest        "nginx"        192.168.0.42:80->80/tcp         node-1      prickly_engelbart

最终，集群拒绝运行容器因为集群中没有 80 端口可用的节点。

    $ docker run -d -p 80:80 nginx
    2014/10/29 00:33:20 Error response from daemon: no resources available to schedule container

## 依赖过滤器（Dependency Filter）

这个过滤器共同调度同一节点中相互依赖的容器。

目前声明有如下依赖：

- 共享卷（Shared volumes）: --volumes-from=dependency
- 链接（Links） --link=dependency:alias
- 共享网络栈（Shared network stack）: --net=container:dependency

Swarm 会尝试在同一节点共同定位依赖容器。如果不能完成(由于相互依赖的容器不存在或者节点没有足够资源), 将会阻止容器创建。

如果可能，多个依赖的组合会被使用。如 --volumes-from=A --net=container:B 会尝试共同定位同一个节点的容器 A 和 B，如果容器在不同节点则调度器不会调度容器。

Health Filter

这个过滤器阻止不健康节点的容器调度。
