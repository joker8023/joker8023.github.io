---
 title: 基于docker 的confluence 安装
 tags: 
	- docker
	- jira
 categories: [docker,jira]
---
##  安装 postgres
```
docker run -p 5434:5432 --name postgres_confluence  -d     -e 'POSTGRES_USER=wiki'     -e 'POSTGRES_PASSWORD=wikijoker'     -e 'POSTGRES_ENCODING=UTF8'     -e 'POSTGRES_COLLATE=C'     -e 'POSTGRES_COLLATE_TYPE=C'     postgres
```
##  安装confluence
```
docker run -d --name confluence --link postgres:postgres_confluenec -p 3090:8090 -p 3091:3091 blacklabelops/confluence:5.9.4
```
##  破解confluence
 
### 复制并备份文件
复制原始文件
```
docker cp confluence:/opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar /home/gench/
```
进入容器 并备份文件

    docker exec -it confluence /bin/sh
    mv /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar  /opt/atlassian/confluence/confluence/WEB-INF/lib/atlassian-extras-decoder-v2-3.2.jar.bak




###  使用破解工具生成新的jar文件
####  confluence5.1-crack文件夹内
删除atlassian-extras-2.4.jar，
####  使用3.1 3.2.2 copy文件的atlassian-extras-decoder-v2-3.2.jar 改成atlassian-extras-2.4.jar
####  进入 iNViSiBLE ./keygen.sh 将记录下来的Service ID 输入，name可以随意填写
####  点击patch选择刚改名的文件点击.gen！生成key，即生成

###  将atlassian-extras-2.4.jar改回atlassian-extras-decoder-v2-3.2.jar并复制回原先目录

```
docker cp /home/gench/atlassian-extras-decoder-v2-3.2.jar confluence:/opt/atlassian/confluence/confluence/WEB-INF/lib/
```
### 重启

```
docker stop confluence
docker start confluence
```