# Nacos
[toc]{type: "ul", level: [2,3]}
## Start
### 启动nacos服务
- 官网下载[nacos](https://nacos.io/)
- 进入bin目录,`sh start.sh -m standalone`启动单服务
- 通过http://127.0.0.1:8848/nacos 访问验证是否启动服务成功

## 融入Spring Cloud作为注册中心
### path1 : discovery
引入依赖
```xml
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
    <version>2023.0.1.2</version>
</dependency>
```
> [!note]
> 注意version

application配置
- application.properties
```properties
#nacos
spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848
```

- application.yml
```yml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: http://localhost:8848
        namespace: public
        group: DEFAULT_GROUP
```
在项目启动类上,注解启动
```java
@SpringBootApplication
@EnableDiscoveryClient
public class SpringdemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringdemoApplication.class, args);
    }

}
```
成功页面
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241215204326.png)

### path2 : nacos config