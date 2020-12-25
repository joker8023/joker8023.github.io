# docker 及其相关

## docker 安装

1.  检测是否有 curl

```
which curl
```

2. 如果没有安装

```
sudo apt-get install curl
```

3. 安装 docker

```
curl -sSL https://get.daocloud.io/docker | sh
```

4. 安装 docker-compose

有两种方式，第一种

```
curl -L https://get.daocloud.io/docker/compose/releases/download/1.11.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

第二种，基于 pip

```
pip install docker-compose
```

## docker 换源

---

换源支持 daocloud，建议支持 daocloud，注册账号并换源，也可以按照一下操作换源

```
curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://ea03381d.m.daocloud.io
```

## 常用命令

### docker

查看 版本

    docker -v

查看镜像

    sudo docker images

删除镜像

sudo docker rmi 镜像 id
删除容器

    sudo docker rm 容器id

杀死所有 running 状态的容器

docker kill \$(docker ps -q)
删除所有已经停止的容器

    docker rm $(docker ps -a -q)

删除所有镜像：

docker rmi \$(docker images -q)
删除 tag 为 none 的镜像

```
docker images|grep none|awk '{print $3}'|xargs docker rmi
```

### docker-compose

启动容器

    docker-compose up

进入容器
例如 postgres

```
docker exec -it atlassian_database_1  psql -U postgres
```

https://bitbucket.org/r.rosario/mayan-edms-docker/raw/4efe54e16538f5b605ad25a20f8560492ca61828/docker-compose.yml
3324480961

```
//docker swarm join --token SWMTKN-1-0y2bju4gl079uypsg04wv0dys3pb67allygatm9kestdv8figr-8lu19fjv0hi7orgyz2yugcnut 192.168.204.41:2377
```

docker run -p 8080:8080 -p 50000:50000 -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
