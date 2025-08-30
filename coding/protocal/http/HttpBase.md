# HttpBase
[toc]{type: "ul", level: [2,3]}
## 概念 
### HTTP
Hyper Text Transfer Protocal(超文本传输协议)
> 传输协议:定义了客户端与服务器端之间发送消息的格式(组成规则)

### 特点
- 基于TCP/IP的高级协议
- 默认端口是80
- 基于请求/响应模型: 一次请求一次响应
- 无状态协议

### 历史版本
- 1.0: 完善的版本,但每次请求响应都会建立新的连接(TCP连接?)
- 1.1: 复用连接、更好的缓存支持等

## 请求消息数据格式
### raw
```
GET /webdemo-1.0-SNAPSHOT/ HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
Cache-Control: max-age=0
Connection: keep-alive
Cookie: JSESSIONID=ADC8752597A5EC5669A9E369DF1C822C; JSESSIONID=5AB16D38D71CCB811EEB884AA5291F7F
Host: localhost:8080
If-Modified-Since: Tue, 10 Dec 2024 04:58:20 GMT
If-None-Match: W/"141-1733806700000"
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: none
Sec-Fetch-User: ?1
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/118.0.0.0 Safari/537.36
sec-ch-ua: "Not=A?Brand";v="99", "Chromium";v="118"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "macOS"
```

### 请求行
定义请求方式
- 格式: 请求方式 请求资源路径 协议/协议版本
- 如果是GET方法,请求参数会拼接在url后(如“?a=1&b=2”作为参数拼接),在请求行中,相对并不安全
```
<!-- 
    GET 是get方法
    /webdemo-1.0-SNAPSHOT/ 请求这个资源
    HTTP HTTP协议
    1.1  HTTP协议1.1
 -->
GET /webdemo-1.0-SNAPSHOT/ HTTP/1.1
```

### 请求头
定义作为客户端的一些请求参数,请求信息
- 每一行代表一个请求参数
- 参数是k-v对格式,通过“:”分隔
- 参数值可以是数组,用“,”分隔
- 重要请求头: 如Cookie
- 可以自定义请求头
```
<!-- 服务器可以解析并拿到请求头name为Cookie的值 -->
Cookie: JSESSIONID=ADC8752597A5EC5669A9E369DF1C822C;
```

### 请求空行
用于区分请求头和请求体的空行
### 请求体
Post方法请求的内容会在请求体中
- 数据格式与请求头中的`Content-Type`和`Content-Length`有关
## 响应消息数据格式
### raw
```
HTTP/1.1 200
Content-Type: application/json;charset=UTF-8
Content-Length: 19
Date: Tue, 10 Dec 2024 09:43:00 GMT
Keep-Alive: timeout=20
Connection: keep-alive
```

### 响应行
对响应的描述
- 格式: 协议/版本 状态码 状态描述
```
HTTP/1.1 200
```

### 响应头
作为响应端对响应的一些信息描述
```
Content-Type: application/json;charset=UTF-8
Content-Length: 19
Date: Tue, 10 Dec 2024 09:43:00 GMT
Keep-Alive: timeout=20
Connection: keep-alive
```
### 响应空行
响应头与响应体之间的间隔空行
### 响应体
响应的消息主体内容
- 内容由响应头中的`Content-Type`、`Content-Length`等确定