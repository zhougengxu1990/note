# clashx verge + docker easyconnect
## 背景
公司使用easyconnect做vpn用于远程电脑,但实际它并不好用,安装完客户端登录之后导致其他网络如网页请求走此代理,网页请求不通

mac上更不好用

自己在mac上使用其他机场代理,目标是公司内网请求使用easyconnect代理,外网请求走外网代理

## docker easyconnect
> 使用docker安装隔离easyconnect,避免出现安装easyconnect客户端后导致的自己网络请求异常情况

[github docker easyconnect](https://github.com/docker-easyconnect/docker-easyconnect)

### 安装
> 原尝试使用纯命令行安装,但配置好用户名和密码之后无法使用,最后安装了图形界面版

安装图形界面版Easyconnect
```bash
docker run --rm --device /dev/net/tun --cap-add NET_ADMIN -ti -e PASSWORD=xxxx -e URLWIN=1 -v $HOME/.ecdata:/root -p 127.0.0.1:5901:5901 -p 127.0.0.1:1080:1080 -d -e DISABLE_PKG_VERSION_XML=1 hagb/docker-easyconnect:7.6.7

```
> [!note]参数说明
> - 1080端口是socks5代理端口
> 5901是vnc请求端口
> PASSWORD是vnc登录时使用的密码
> `-d`后台运行

>[!tip]额外参数
> - arm64 和 mips64el 架构需要加入 `-e DISABLE_PKG_VERSION_XML=1` 参数
> - 如果需要http代理 `-p 127.0.0.1:8888:8888`

docker使用run直接运行,优先在本地查找image,如果没找到,自动下载,下载完成后再启动.

`docker update --restart=always [containerId]`设置容器与docker启动后也自动启动.也可以在run参数中添加

### vnc登录Easyconnect
需要下载VNC客户端

Mac自带,浏览器输入 `vnc://127.0.0.1:5901`会自动识别打开 屏幕共享.app

密码就是docker运行容器设置的密码xxxx  `PASSWORD=xxxx`

![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250406200605.png)

使用easyconect的账号和密码登录

> [!warning]遇到的问题
> 1.如果出现黑屏，大概率是docker版本问题
> 2.如果连不上，检查服务是否正常启动，查看docker日志

<br/>

## 代理配置
### clashx verge
为什么我使用clashx verge,其实我一直在用clashx,clashx没有tun模式,使用clashx verge可以开启tun模式.cfw(clash for win)也有tun模式

![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250406202239.png)

### 代理规则配置

配置代理规则,将内网请求转发到socks://127.0.0.1:1080 (easyconnect)代理请求

clashx verge 有 全局扩展覆写配置和全局扩展脚本,可以对clashx verge代理进行全局配置,选择自己使用最熟悉最方便的方式,灵活性比较高

![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250406202607.png)

个人优先使用了规则配置,配置了proxies和rules,由于不熟悉配置或其他原因,规则配置没有生效

最后使用了脚本配置,成功生效,达到了自己的代理要求和目的
- 公司内网使用Easyconnect代理
- 外网使用外网代理

```js
// 全局扩展脚本
function main(config, profileName) {
  const prependRule = [
    'IP-CIDR,192.168.120.0/24,EASY'
  ];
  const prependProxy = [
    { name: 'EASY', type: 'socks5', server: '127.0.0.1', port: '1080'}
  ]
  config['rules'] = [...prependRule,...config['rules']]
  config['proxies'] = [...prependProxy,...config['proxies']]
  return config;
}


```

