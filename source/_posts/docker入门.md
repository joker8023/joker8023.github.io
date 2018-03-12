---
 title: docker入门
 tags: 
	- docker
---
## docker安装
1.检测是否有curl

``` 
which curl
```

2.如果没有安装

```
sudu apt-get install curl
```
3.安装docker

```
curl -sSL https://get.daocloud.io/docker | sh
```
4.安装 docker-compose
 
有两种方式，第一种

```
curl -L https://get.daocloud.io/docker/compose/releases/download/1.11.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

第二种，基于pip

```
pip install docker-compose
```
## docker 换源

---
换源支持daocloud，建议支持daocloud，注册账号并换源，也可以按照一下操作换源
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
    
    sudo docker rmi 镜像id
删除容器

    sudo docker rm 容器id
杀死所有running状态的容器
    
    docker kill $(docker ps -q)
删除所有已经停止的容器

    docker rm $(docker ps -a -q)
删除所有镜像：
    
    docker rmi $(docker images -q)
    
### docker-compose
启动容器

    docker-compose up 
进入容器
例如postgres

```
docker exec -it atlassian_database_1  psql -U postgres
```