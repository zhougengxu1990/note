# Linux游戏适配
## 原生支持linux
直接运行

## window游戏
安装steam,在steam中添加游戏，并以兼容proton模式运行

### 老游戏遇到的问题
*第一次打开游玩正常但第二次打不开了*
这是 DXVK（Vulkan）在你的 AMD 760M / RADV 驱动环境下，处理《爱丽丝》这个老旧 DX9 游戏的初始化逻辑时发生了“状态死锁”或驱动段错误。
```
# 在游戏启动项中添加如下配置，以wine opensl方式,而不是vulkan
PROTON_USE_WINED3D=1 %command%
```