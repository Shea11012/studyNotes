在 centos 开启 firewalld ，使用 docker 访问时报错 nginx 502

解决方法：

1. 关闭 firewalld

```sh
sudo systemctl stop firewalld
sudo systemctl disable firewalld
```

2. 不关闭 firewalld 解决办法

```sh
firewall-cmd --permanent --zone=trusted --change-interface=docker0
firewall-cmd --permanent --zone=trusted --add-port=端口号/[tcp|udp]
firewall-cmd --reload 
# 如果不使用 --permanent（常驻进程）不需要 reload
```

firewall 添加完成后需要重启 docker，如果报了如下错误：

```sh
Failed to Setup IP tables: Unable to enable SKIP DNAT rule:  (iptables failed: iptables --wait -t nat -I DOCKER -i br-9b0f1d61a45a -j RETURN: iptables: No chain/target/match by that name.(exit status 1)
# 需要检查 iptables 是否有docker chain，iptables 重启会导致 docker chain 丢失

#解决
iptables -L -n -t nat | grep DOCKER
systemctl restart docker
```

