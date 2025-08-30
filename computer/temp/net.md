# net
### 运行一个简易 TCP 服务
在 CentOS 上直接运行一个简易 TCP 服务
```bash
# 在 CentOS 上启动临时 TCP 服务监听 1080 端口
sudo nc -klv 0.0.0.0 1080

```
用途：可用于确认一个服务器端口是否可达
```BASH
# cmd
telnet ip port

# powershell
Test-NetConnection -ComputerName 192.168.101.95 -Port 1080

```
