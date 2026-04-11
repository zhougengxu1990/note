# Linux localSend

可能由于防火墙的原因，其他设备看不到linux上的localSend应用设备


```
# 有的防火墙使用firewall-cmd,有的用ufw需要进行验证
# 查看utw状态，当`status: active`表示可用
sudo utw status


# 开放端口
sudo ufw allow 53317/tcp
sudo ufw allow 53317/udp

```