# Docker
“com.docker.vmnetd”将对你的电脑造成伤害
```sh

# 停掉 Docker 服务
sudo pkill '[dD]ocker'

# 停掉  vmnetd 服务
sudo launchctl bootout system /Library/LaunchDaemons/com.docker.vmnetd.plist

# 停掉 socket 服务
sudo launchctl bootout system /Library/LaunchDaemons/com.docker.socket.plist

# 删除 vmnetd 文件
sudo rm -f /Library/PrivilegedHelperTools/com.docker.vmnetd

# 删除 socket 文件
sudo rm -f /Library/PrivilegedHelperTools/com.docker.socket
```

请求的服务无法连接或被积极拒绝，查看一下日志
```bash
docker logs [container_id]

```
### 安装docker 


卸载旧版本

```bash
yum remove docker docker-common container-selinux docker-selinx docker-engine


```
添加docker-ce.repo
```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo


```
yum 更新索引
```bash
# centos7
yum makecache fast

```
安装
```bash
yum install -y docker-ce

```

### docker设置代理镜像站

修改/etc/docker/daemon.json
```json
    "registry-mirrors":[
        "https://docker.1ms.run"
    ]

```


### 出现的问题

> [!warning]Failed to start docker.service: Unit not found.
> 启动docker时报错，有可能是安装多版本，需要完全卸载

> [!warning]centos安装docker显示 No package docker-ce available
> 需要添加docker-ce.repo

> [!warning]端口被占用docker: Error response from daemon: Ports are not available: exposing port TCP 0.0.0.0:1081 -> 127.0.0.1:0: listen tcp 0.0.0.0:1081: bind: An attempt was made to access a socket in a way forbidden by its access permissions.
> 实际未被占用，
> ```
> # windows停止nat 服务。。
> net stop winnat