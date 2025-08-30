# mac使用技巧
[toc]{type: "ul", level: [2,3]}
## 快捷键
### f功能系列
```
f1  启动台
f5  刷新,其实比较少使用
f11 桌面
f12 chrome调试
```
### 软件
| 软件 | 快捷键 | 说明 |
| -- | -- | -- |
| Alfred | `alt + space` | 查找app和文件(先输个“空格”) |

<br>

## 系统优化或个性化
### 更改系统截图的保存位置
```sh
# 最后的参数是保存位置,自定义
defaults write com.apple.screencapture location ~/screencapture
```
### 启动台的快捷键设置
```
设置-->键盘-->启动台和扩展坞-->☑️显示启动台-->输入快捷键-->完成
```
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-%E6%88%AA%E5%B1%8F2024-06-29%2010.48.48.png)

### 开启fps
```sh
# 开启
/bin/launchctl setenv MTL_HUD_ENABLED 1
# 关闭
/bin/launchctl setenv MTL_HUD_ENABLED 0
```
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241219145041.png)

## 使用技巧
### 方向键翻动同一目录的图片
`选中`一个图片-->按下`空格`-->`上下键`可以翻动其他图片
### 删除._开头的文件
```sh
find . -name "._*" -type f -delete
```

<br/>

## 虚拟机
### 虚拟机安装系统后没有网络
- `虚拟机` --> `安装VMware Tool`
- cmd中输入 `Set-ExecutionPolicy RemoteSigned` 命令
- 在D盘下,执行`.setup`命令，开始 VMware Tools 的安装
- 重启虚拟机