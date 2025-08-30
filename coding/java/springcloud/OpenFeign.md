# OpenFeign
[toc]{type: "ul", level: [2,3]}
## 介绍
[官网](https://springdoc.cn/spring-cloud-openfeign/)
### 描述
### 谁开发的
### 解决了什么问题
### 适用场景
## Start
### 引入
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
<!-- spring cloud高版本时,使用微服务调用时,需要loadbalancer. -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-loadbalancer</artifactId>
    <version>3.0.0</version>
</dependency>
```
> [!important]spring-cloud-starter-loadbalancer
> spring cloud高版本弃用了ribbon
### 代码
Spring开启使用
```java
// @EnableFeignClients

@SpringBootApplication
@EnableFeignClients
public class SpringdemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringdemoApplication.class, args);
    }

}

```

创建feign服务提供接口
```java
/** 
 * 1.使用@FeignClient注解标在接口上
 * 2.name指定微服务名称
 * 3.url指定服务url,url和name同时设置时,url生效
 * 4.path在请求路径前加此path
 * 5.接口方法支持mvc注解
 */
@FeignClient(name = "micro-service-name",url = "127.0.0.1:8081",path = "")
public interface FeignTestService {

    @RequestMapping("/data")
    public Object data(@RequestParam Integer a);
}

```
调用feign的代码,注入接口,直接调用
```java
@Autowired
private FeignTestService feignTestService;

@RequestMapping("/feign")
public Object feign(@RequestParam Integer a){
    return feignTestService.data(a);
}

```
### 配置
配置类
```java
@Configuration
public class FeignConfiguration {

    /**
     * 配置decoder,对请求返回数据进行解码工作
     * @return
     */
    @Bean
    public Decoder feignDecoder() {
        return new ResponseEntityDecoder(new SpringDecoder(feignHttpMessageConverter()));
    }

    private ObjectFactory<HttpMessageConverters> feignHttpMessageConverter() {
        HttpMessageConverters httpMessageConverters = new HttpMessageConverters(new StringHttpMessageConverter(),new GateWayMappingJackson2HttpMessageConverter());
        return () -> httpMessageConverters;
    }

    static class GateWayMappingJackson2HttpMessageConverter extends MappingJackson2HttpMessageConverter{
        public GateWayMappingJackson2HttpMessageConverter() {
            List<MediaType> mediaTypes = new ArrayList<>();
            /**
             * 指定支持text/html和application/json的context-type,并转换处理为object对象
             */
            mediaTypes.add(MediaType.TEXT_HTML);
            mediaTypes.add(MediaType.APPLICATION_JSON);

            setSupportedMediaTypes(mediaTypes);
        }
    }
}
```

### 遇到的问题
> [!caution]No Feign Client for loadBalancing defined
> [spring cloud 高版本ribbon被弃用](#引入),需要引入spring-cloud-starter-loadbalancer


## 扩展功能
### 拦截器
拦截器传递上游服务Header,主要应用是传递用户登录态
```java
@Component
public class FeignRequestInterceptor implements RequestInterceptor {
    @Override
    public void apply(RequestTemplate template) {
        ServletRequestAttributes requestAttributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        if(requestAttributes != null){
            Enumeration<String> headerNames = requestAttributes.getRequest().getHeaderNames();
            String headerName;
            while (headerNames.hasMoreElements()){
                headerName = headerNames.nextElement();
                if("content-type".equalsIgnoreCase(headerName)){
                    continue;
                }
                template.header(headerName,requestAttributes.getRequest().getHeader(headerName));
            }
        }
    }
}


```

## 特性
## 组件
## 实现原理
## 优点与不足