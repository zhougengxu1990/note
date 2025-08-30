# Steam使用第三方2FA
### 背景
Steam令牌只能使用自己的mobile app,实际很不方便.如果能用第三方2FA就好了,统一管理
可以以手动输入密钥的方式添加,关键在于获取密钥
目前比较方便的是以下两种方式: 
- 桌面端验证器(WinAuth、SDA等),登录激活后导出TOTP Secret
- 手机root, 使用官方App登录完成验证激活后获取

<br/>

### 使用SDA获取TOTPSecret
下载SteamDesktopAuthenticator([SDA](https://github.com/Jessecar96/SteamDesktopAuthenticator))

按流程完成激活
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250216010546.png =300x)

在软件安装目录`maFiles`下有一个`.maFile`文件
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20250216011226.png)

内容格式如下,其中`TOTP_secret`就是我们需要的结果
```
{"shared_secret":"[some_code]","serial_number":"[some_code]","revocation_code":"[some_code]","uri":"otpauth://totp/Steam:[your_Steam_login]?secret=[TOTP_secret]&issuer=Steam","server_time":1569762545,"account_name":"[your_Steam_login]","token_gid":"1429bb44bf725072","identity_secret":"[some_code]","secret_1":"[some_code]","status":1,"device_id":"android:[some_code]","fully_enrolled":true,"Session":{"SessionID":"[some_code]","SteamLogin":"[some_code]","SteamLoginSecure":"[some_code]","WebCookie":"[some_code]","OAuthToken":"[some_code]","SteamID":[some_code]}}
```

<br/>


### 导入第三方2FA
手动输入TOTP Secret添加,注意下面的OTP身份验证方式选择`Steam`
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/IMG_0952.PNG =300x)
完成
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/IMG_0953.PNG =300x)

<br/>

### 保存备份文件
- 备份.maFile文件,最重要的是里面有一个`revocation_code`(恢复码,通常是R开头),当不需要令牌验证器时可以申请移除需要用恢复码
- 备份备用 Steam 令牌验证码,可以由页面端获取,一次申请30个