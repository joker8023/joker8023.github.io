
---
title: ubuntu换源
tags: 
	- ubuntu
---
## 1：查看自己的ubuntu系统的codename

```
 lsb_release -a
```
显示如下

```
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.2 LTS
Release:	14.04
Codename:	trusty

```
## 2：确认阿里源支持
登录[http://mirrors.aliyun.com/ubuntu/dists/](http://mirrors.aliyun.com/ubuntu/dists/)
查看ubuntu的源支持
## 3 备份系统源


```
cd /etc/apt
sudo mv sources.list sources.list_bak
```
## 4添加新的源文件：
```
    sudo vi sources.list
```
并添加以下内容：注意，每一行的trusty应该用第一步查看得到的Codename来代替
```
deb http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe
deb http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-backports main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-proposed main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-security main multiverse restricted universe
deb-src http://mirrors.aliyun.com/ubuntu/ trusty-updates main multiverse restricted universe
```
## 5. 更新源
```
sudo apt-get update
```