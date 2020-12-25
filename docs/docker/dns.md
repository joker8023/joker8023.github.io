# docker 配置 DNS

## 关闭系统的 systemd-resolved

```
 systemctl disable systemd-resolved
 systemctl stop systemd-resolved
```

## 运行 dnsmasq

```


docker run -d --restart=always --name dns-server -p 53:53/tcp -p 53:53/udp --cap-add=NET_ADMIN -v /usr/share/dnsmasq:/etc/dnsmasq  andyshinn/dnsmasq
```

## 配置 DNS 服务

### 为 dnsmasq 配置一个真正的 dns 服务器地址

```

vim /usr/share/dnsmasq/resolv.conf

# 这里我使用的是阿里云的DNS
nameserver 223.5.5.5
nameserver 223.6.6.6
```

### 配置本地解析规则

```

vim /usr/share/dnsmasq/dnsmasqhosts

# 这里配置自定义解析
127.0.0.1 master
```

### 修改 dnsmasq 配置文件，指定使用上述两个我们自定义的配置文件

```
docker exec -it dns-server /bin/sh

vi /etc/dnsmasq.conf

# 修改下述两个配置
resolv-file=/etc/dnsmasq/resolv.conf
addn-hosts=/etc/dnsmasq/dnsmasqhosts
```

### 回到宿主，重启 dns-server 容器服务

```
docker restart dns-server
```
