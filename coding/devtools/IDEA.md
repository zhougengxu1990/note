# IDEA
[toc]{type: "ul", level: [2,3]}
## debug

### debug
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241129155911.png)

### remote debug
在Configurations选择Remote JVM Debug
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241129161944.png)

配置如下.(3)是由(1)和(2)生成的服务端开启remote debug的jvm 参数.可以复制配置到服务器的vm启动参数开启
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241129161842.png)

服务端开启remote debug的配置
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241129162304.png)
或通过java命令启动
```shell
java -jar -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:9527 xxx.jar --server.port=8081
```

## 推荐插件
### GitToolBox
### Lombok
> Java库，消除构造、getter、setter和equals等方法，使代码简洁
### MybatisX
- Java和Xml文件来回跳转
- Mapper接口文件中定义方法后在xml文件中生成相关代码
- 连接数据库后自动生成dao、service、serviceImpl、mapper.xml等

### Maven Hepler
> 依赖分析
- 方便找到和排除冲突依赖

### GsonformatPlus-JSON
> Json->对象类代码生成

### SonarLint
> 代码质量管理

