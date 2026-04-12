# Linux安装CachyOS
## 关于CachyOS
### 介绍
基于 Arch Linux 的衍生系统,CachyOS 从设计之初便以为用户提供更出色的运行速度、更可靠的安全性与更便捷的使用体验为核心目标，既继承了 Arch Linux 滚转更新、高度可定制的核心优势，又通过全链路的性能优化，解决了原生 Arch Linux 安装繁琐、需手动优化的痛点。

### 选择CachyOS的原因
- 追求极致、高性能、最新的功能，而非保持稳定
- 安装简单
- 开箱即用，内置功能丰富强大，如fd,paru等

### 安装参考
[[软件] 从Windows 11迁移到CachyOS：一个意外顺滑的Linux桌面体验](https://www.chiphell.com/forum.php?mod=viewthread&tid=2731268&extra=page%3D1&page=1)
[CachyOS 安装与设置超详细教程（包括双系统配置）](https://blog.drxian.cn/archives/715)

<br/>

## 准备
### 下载
官网下载，选择桌面版本

http://cachyos.org/

### 写入u盘
准备一个u盘

使用写入工具写入，WINDOWS 可以使用 RUFUS

<br/>

## 安装步骤
### u盘启动
正常从u盘启动后（如果没有正常启动，需要设置BIOS启动优先级从U盘启动，或如果是WINDOW系统可以从 设置【恢复】->【高级启动】，从设备启动）

如果进入cachyOS live界面，说明成功

### 设置镜像源
如果不设置镜像源，安装过程中会连网从镜像源下载部分包进行安装，连接下载超时或不可访问，会直接导致安装失败

```
# 如果执行不成功，直接手动写入各文件
sudo bash -c 'echo "Server = https://mirrors.ustc.edu.cn/cachyos/repo/\$arch/\$repo" > /etc/pacman.d/cachyos-mirrorlist'
```
### 开始安装
- live系统连网

- 进入live会自动打开CachyOSHello程序，左正角可以设置语言为简体中文，选择系统语言、时区：Asia、Shanghai、分区、引导、选择桌面、账号密码，一直到install。【先不要install，见下条】

- 注释掉update-mirrorlist内容
    > 注释掉可以使得在安装过程中不会执行更新上面已经设置好的镜像源，更新镜像源也会导致安装失败

    ```
    # 这个命令会在文件每一行前添加#实现内容注释的效果
    # 如果没有执行成功，可以手动注释
    sudo sed -i 's/^/#/' /etc/calamares/scripts/update-mirrorlist
    ```

- 继续install进行安装，只要镜像源正常，基本可以直接安装成功

<br/>

## 系统基本设置及优化
### 添加 Arch Linux 中文社区仓库
在/etc/pacman.conf末尾添加，如果换其他参考：[repo列表](https://github.com/archlinuxcn/mirrorlist-repo)
```
[archlinuxcn]
Server = https://mirrors.cloud.tencent.com/archlinuxcn/$arch
```

添加完成后导入密钥
```
sudo pacman -Sy && sudo pacman -S archlinuxcn-keyring
```

### 软件检索和安装
使用AUR(Arch User Repository)助手paru,paru基于yay，可帮助 Arch Linux 及其衍生系统用户方便地搜索、安装、升级和管理 AUR 软件包，同时封装了 pacman 的功能，交互简洁高效。
```
# 搜索
sudo paru -Ss xxx
# 安装
sudo paru -S xxx
# 卸载
sudo paru -R xxx
# 清理缓存（官方包的残余）
suod pare -Sc
# 清理编辑缓存（如果有空间可以不清理cache,有利于增量更新，否则可以清除）
rm -rf ~/.cache/paru/clone/软件名
```
### 输入法

<br/>

## 注意事项
### 使用cachyos-rate-mirrors更新镜像源
这是cachyOS官方更新镜像源的方式
```
sudo cachyos-rate-mirrors
```
但是不建议！

这条命令会自动更新延迟低的镜像源到mirrorlist中，但实际测试，不如上面手动配置国内镜像源直接成功，反而会导致安装失败（ping是没问题的，原因未知）

### 不要提前注释掉update-mirrorlist内容
一定要在install之前的那个节点进行注释

如果提前进行注释，update-mirrorlist的内容会重新生成，你的注释就失效了,导致安装失败