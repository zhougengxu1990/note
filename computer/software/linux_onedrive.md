# Linux Onedrive
## 选择与安装
```
# 推荐直接使用纯cli onedrive安装
paru -S onedrive-abraunegg
```
可能会提示你选择安装哪个包，选择第1个，对应 LDC (LLVM-based D Compiler)。
```
:: 有 2 个软件包可提供 d-runtime ：  
:: 软件仓库 extra
   1) liblphobos  2) libphobos
```


## 使用
###  授权登录
安装成功后，执行以下cli进行授权登录操作
```
onedrive
```
- 终端会输出一个很长的 Authorize URL。
- 将该链接复制到浏览器中登录你的 OneDrive 账号
- 登录成功后，浏览器会显示一个空白页面。不要关掉它，直接复制浏览器地址栏里的 全路径 URL
- 回到终端，在 Enter the response uri: 提示后粘贴该 URL 并回车.url如下
    ```
    https://login.microsoftonline.com/common/oauth2/nativeclient?code=M.C535_BL2.2.U.Dj20ayLruQXAUQy2pi4p!rUW7el5SYhT8H3cNWD2xr7eYCg5BnBQ5iZL9tWrVOqRFtAYJL06ho7qWidmRoP0Fbjx8hHDWv*tTNxRt0Ya8DBECKVhdlpM9KT7h!yErvWFiYvINU46pUPfay1Zx3qOYc1WgVnky!4NMrj3ERFNN3uGdhwkhPMeemDJitRXn91UOkHWqpuOzGzX7hxu9rhsAW6s0hcnjDWMtGll4W*biNhmtC*BNa1Za0Gj8Qn8SdS12n1kI8YKelt9iR8UplMEj9O3kaQhhkn*Ef*eBAr1eq1v!ma!R5AxsBl!dlOIUt04nMe9znTCbLOFDycjBfARjd9en9PMHmIo3aEmCGad2FjYuun7CihNV1H53BMQcDMbxhtZiYpAniU2QUSFtPnM*iE*Ck1cQyODqPhgK!vWS*sTVweN0EwBoweWA7IzLx!DQ40Tb5yrH5OHAJtmTffilsZHqUiCG*g0GFJE5J9koVQ7PO8kmuo*iRMgjH9ig9ghdzfXcP664RdPDlqJcLk7p0i1hbdwD!0At6h9e!NEXcY*BT1fhA*JGBIni4kI*F!WYDmom8ku23qQCi9SiW3BsEo%24
    ```

### 同步
手动同步，这个同步是双向的（云端和本地之间）
```
# 第一次同步
onedrive --sync
```
第一次成功同步会创建`~/OneDrive`目录，所有文件都在这个目录

### 设置
```
# 1. 允许服务开机自启
systemctl --user enable onedrive

# 2. 立即启动服务
systemctl --user start onedrive
```

## 注意事项
### 找到正确的授权成功后的URL
在授权阶段，有时有可能在页面登录成功后，页面最终跳转到一个wrong page,并不你登录或其他的错误原因。我们需要的URL是登录成功后的第一个跳转的链接，及时按esc终止页面跳转，即可拿到正确的需要的URL（通常这个URL很长）