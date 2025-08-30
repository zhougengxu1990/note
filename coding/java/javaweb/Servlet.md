# Servlet
[toc]{type: "ul", level: [2,3]}
## 概念
### 什么是Servlet
- Servlet是server applet缩写,意为运行在服务器的小程序
- JavaEE规范之一,Servlet是一个定义动态资源的接口,被web服务器(如tomcat)托管并被识别和调用
### Servlet请求流程图
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241210131724.png)
- 1)客户端Client向服务器发起请求
- 2)请求是向web服务器请求,如tomcat的默认8080端口
- 3)web服务器对请求进行分析,解析被请求的资源,通过ServletMapping找到对应的Servlet
- 4)调用实现的Servlet,如果未创建Servlet会先创建Servlet(此处和web服务器配置有关,若配置服务器启动时就创建Servlet此时Servlet已经存在对象)
- 5)执行Servlet中的service方法,执行程序员写的代码逻辑内容
- 6)response返回结果
- 7)web服务器向client返回Response
- 
- 综上,也可以看出,Servlet定义了web服务器请求动态资源的接口,定义了接口规范,web服务器由此可以方便调用实现了的Servlet

## Start
### web.xml配置的方式使用Servlet
流程
- 创建JavaEE项目
- 定义一个类并实现Servlet接口
- 在web.xml中配置Servlet

Servlet实现
```java
public class ServletDemo1 implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("ServletDemo1 init...");
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("ServletDemo1 service...");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

web.xml配置Servlet
> servlet-class配置的是servlet实现类的全类名,一看到全类名就应该想到会使用到反射,web服务器就是通过反射创建servlet对象,要求实现的servlet类中需要有无参的构造方法
```xml
<!--    配置servlet-->
    <servlet>
        <servlet-name>demo1</servlet-name>
        <servlet-class>cn.zhougx.demo.webdemo.ServletDemo1</servlet-class>
    </servlet>
<!--    配置servlet映射,用于web服务器请求识别-->
    <servlet-mapping>
        <servlet-name>demo1</servlet-name>
        <!-- 配置的资源访问路径 -->
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>

```
当访问http://localhost:8080/contextpath/demo1 时实现对servlet的访问

### Servlet 3.0
> Servlet 3.0开始支持注解配置Servlet,不需要在web.xml中进行配置,可以不创建web.xml; 定义一个类,实现Servlet接口; 在类中使用@WebServlet注解配置

流程
- 创建JavaEE项目,选择Servlet 3.0以上版本
- 在Servlet的实现类上使用注解@WebServlet,默认值value就是servlet path
```java
@WebServlet("/demo2")
public class ServletDemo2 implements Servlet {
    //...
}

@WebServlet("/hello-servlet")
public class HelloServlet extends HttpServlet {}
```
### 遇到的问题
> [!caution]servlet not found,可能是servlet与tomcat的版本不兼容问题
使用了import jakarta.servlet.http.*;
The Jakarta EE platform is the evolution of the Java EE platform. Tomcat 10 and later implement specifications developed as part of Jakarta EE. Tomcat 9 and earlier implement specifications developed as part of Java EE.

## Servlet生命周期
- 创建对象,并初始化执行init方法(对象只创建一个,所以init只执行一次)
- 请求调用,执行service方法
- web服务器停止结束时,执行destroy方法,销毁对象

> Servlet实现对象在web容器中是单例的,需要注意线程安全问题
```java
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {
         
    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }

```
## Request
> 1.web服务器如Tomcat会创建Servlet对象
> 2.web服务器同时创建Request和Response对象,将请求参数封装进Request对象中
> 3.将Request和Response对象一起传给Servlet供其使用

### Request继承体系结构
```
<!-- servletRequest接口 -->
jakarta.servlet.ServletRequest
    |
    <!-- http servletRequest接口 -->
    jakarta.servlet.http.HttpServletRequest
        |
        <!-- tomcat服务器实现的Request对象 -->
        org.apache.catalina.connector.RequestFacade
```

### 获取请求消息数据[http请求消息数据格式](../../protocal/http/HttpBase.md#请求消息数据格式)
```java
//获取请求行
String method = request.getMethod();
String contextPath = request.getContextPath();//获取虚拟路径
String servletPath = request.getServletPath();//获取servlet路径
String requestURI = request.getRequestURI();
StringBuffer requestURL = request.getRequestURL();
String protocol = request.getProtocol();
System.out.println(method + " " + requestURI + " " + protocol);

//获取请求头
Enumeration<String> headerNames = request.getHeaderNames();
String headName;
while (headerNames.hasMoreElements()){
    headName = headerNames.nextElement();
    System.out.println(headName + ":" + request.getHeader(headName));
}
System.out.println();

//字节流请求体
BufferedReader reader = request.getReader();
String line = null;
while ((line = reader.readLine())!= null){
    System.out.println(line);
}


//客户端ip
String remoteAddr = request.getRemoteAddr();
System.out.println(remoteAddr);


<!-- 以下为输出 -->
POST /webdemo/hello-servlet HTTP/1.1
host:localhost:8080
user-agent:curl/8.1.2
accept:*/*
content-length:11
content-type:application/x-www-form-urlencoded

name=zhougx
127.0.0.1
```

### 其他功能
- 获取请求参数通用方式
- 请求转发
- 共享数据
- 获取ServletContext
```java
//获取请求参数通用方法
String name = request.getParameter("name");
System.out.println(name);

//请求转发,服务器内部行为,效率高.与forward对应的是redirect重定向,重定向是response行为
try {
    request.getRequestDispatcher("/demo2").forward(request,response);
} catch (ServletException e) {
    throw new RuntimeException(e);
}
```

## Response
可以通过Response设置响应消息数据