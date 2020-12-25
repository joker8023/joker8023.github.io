# ocserv 配置安装

```
https://blog.csdn.net/y87329396/article/details/48264731


wget https://raw.githubusercontent.com/travislee8964/ocserv-auto/master/ocserv-auto.sh
chmod +x ocserv-auto.sh
 ./ocserv-auto.sh
 #添加用户
 ocpasswd -c /etc/ocserv/ocpasswd joker
 #开机启动
 sudo systemctl enable ocserv
 # 开启
 sudo systemctl start ocserv

 sudo ocserv -c /etc/ocserv/ocserv.conf -f -d 1
 # 关闭
  sudo systemctl stop ocserv

```
