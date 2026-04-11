# iRingo WeatherKit
解决iPhone大陆天气数据不准确的问题
[iRingo WeatherKit](https://nsringo.github.io/guide/Weather/weather-kit.html)
- 解锁全部天气数据类型
- 替换「空气质量」数据
- 添加「未来一小时降水强度」信息
- 替换「天气」数据
    - 包括「当前天气」、「每小时天气预报」、「10日天气预报」

## 安装
> 以shadowrocket举例

### 一键安装module
使用手机浏览器打开上面链接，找到【点击一键安装】，自动以module的方式安装到对应的代理工具中（支持loon,surge,shadowrocket,qx等）

![IMG_1333](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/IMG_1333.PNG)


### 配置https解密
因为对https://weatherkit.apple.com的请求是https请求，需要配置并开启，下面有个Certificate受信证书需要安装到系统中并激活
![IMG_1334](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/IMG_1334.PNG)

### 代理需要常开
iPhone自带天气有一定的缓存，天气数据准确和及时性恢复到应有的水平