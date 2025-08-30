# NexusStart
快速入门使用
[toc]
## 介绍

Nexus搭建私服

### 好处
- 加速公网代码库的下载(下载一次缓存在私服)
- 加速团队代码构建(局域网)
- 统一代码管理,版本控制
- 方便上传第三方库
- 私服代码安全

<br/>

## 安装和使用
使用docker安装

### docker安装和启动
docker pull
```bash
docker pull sonatype/nexus3

# 若无法拉取或镜像中不存在,可以指定镜像源pull
# 	docker pull swr.cn-north-4.myhuaweicloud.com/ddn-k8s/docker.io/sonatype/nexus3 (或以此种指定镜像源的方式pull)

```
创建持久化文件目录
> docker run时-v进行映射
```bash
# linux env
mkdir -p /home/nexus/data
chmod 777 /home/nexus/data

# 其他系统创建合适的目录
```


docker run 
```
docker run \
-d \
--name nexus3 \
-p 8081:8081 \
--restart always \
-v /Users/zhougengxu/data/nexus/data:/nexus-data \
sonatype/nexus3
```

### 登录
> 如果是虚拟机使用docker启动，需要firewall开放tcp 8081端口，才能让虚拟机外部访问
> 
请求 `localhost:8081`

![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250406220336.png)

默认用户名是admin
默认密码在 `/nexus-data/admin.password`文件中,bash查看,当然也直接通过本地文件查看(docker启动时已经对nexus-data做了目录映射)

![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250406220806.png)


第一次登录成功需要重置密码
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250406221253.png)
