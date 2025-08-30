# Maven
<!-- TOC tocDepth:2..3 chapterDepth:2..6 -->

- [Introduction](#introduction)
- [Start](#start)
    - [下载](#下载)
    - [settings.xml配置](#settingsxml配置)
    - [仓库配置](#仓库配置)
    - [IDEA配置](#idea配置)
- [Pom文件](#pom文件)
    - [xml & project](#xml-project)
    - [parent](#parent)
    - [当前项目](#当前项目)
    - [properties属性](#properties属性)
    - [dependencyManagement依赖管理](#dependencymanagement依赖管理)
    - [dependencies依赖配置](#dependencies依赖配置)
    - [build](#build)
    - [distributionManagement](#distributionmanagement)
    - [repository](#repository)
- [仓库](#仓库)
    - [分类](#分类)
    - [配置](#配置)
- [Command](#command)

<!-- /TOC -->
## Introduction
是一个项目build工具
是一个项目jar依赖管理工具(通过pom.xml文件管理)

## Start
### 下载
[Maven官网](https://maven.apache.org/download.cgi)
> [!note]maven历史版本下载
> https://archive.apache.org/dist/maven/maven-3

### settings.xml配置
```xml
<!-- 配置阿里云镜像,加速作用 -->
<mirror>
    <id>aliyun</id>
    <name>aliyun</name>
    <mirrorOf>central</mirrorOf>
    <url>http://maven.aliyun.com/repository/public/</url>
</mirror>

<!-- 配置本地仓库路径,下载的jar包会下载到此处 -->
<!-- 若未配置,默认为 ~/.m2/repository -->
<localRepository>/path/to/local/repo</localRepository>
```
### 仓库配置
### IDEA配置
Settings--Maven
- home path
- settings
- repository

![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241215163034.png)


## Pom文件
### xml & project
最外层是\<project></project>标签
```xml
<!-- 声明xml文件格式以及utf-8编码 -->
<?xml version="1.0" encoding="UTF-8"?>

<!-- 项目 -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
         <!-- pom版本 -->
    <modelVersion>4.0.0</modelVersion>
</project>
```
### parent
声明依赖的父工程
```xml
<!-- 父项目 -->
<parent>
    <!-- 分组 -->
    <groupId></groupId>
    <!-- 项目或模块名 -->
    <!-- 分组和项目模块名共同构成唯一性 -->
    <artifactId></artifactId>
    <!-- 版本 -->
    <version></version>
    <!-- parent的pom文件的路径 -->
    <!-- 1.默认我们不用写<relativePath>，那默认值就是 ../pom.xml -->
    <!-- 2.若设置空值,<relativePath/>,则从仓库中获取 -->
    <!-- 3.从指定path获取 -->
    <relativePath/> <!-- lookup parent from repository -->
</parent>

```
### 当前项目
```xml
<groupId>com.example</groupId>
<artifactId>springdemo</artifactId>
<version>0.0.1-SNAPSHOT</version>
<name>springdemo</name>
<description>springdemo</description>
```
### properties属性
配置的属性可在pom文件其他标签中引入使用
```xml
<properties>
    <!-- 配置的java版本 -->
    <java.version>17</java.version>
    <!-- 配置的spring cloud版本 -->
    <spring-cloud.version>2023.0.1</spring-cloud.version>
</properties>
```
> [!tip]
> 使用属性的方式,"\$\{property标签名}"
> 如: \<version>${spring-cloud.version}</version>
### dependencyManagement依赖管理
用于项目管理统一版本,子项目引入依赖时不需要指定version,version与父项目保持一致
此处只是声明依赖,并不实现自动引入
> [!note]
> 一般声明于父依赖中,用于项目版本控制
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <!-- 当此处version变更时,可全局管理和变更版本 -->
            <version>5.1.44</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```
### dependencies依赖配置
配置的依赖会引入项目中
- 依赖的下载路径在maven的本地repo中
- 若本地maven repo中没有会先下载

```xml
<!-- 依赖配置 -->
<dependencies>
    <!-- 其中的一个依赖 -->
    <dependency>
        <!-- 分组 -->
        <groupId></groupId>
        <!-- 项目或模块 -->
        <artifactId></artifactId>
        <!-- 版本 -->
        <version></version>
        <!-- 引入的类型,分为jar,war,pom,没有此项不指定默认为jar -->
        <type></type>
        <!-- 同一版本下的分类指定 -->
        <classifier>jdk17</classifier>
        <!-- 设置当前依赖是否可选. 对子类的影响-->
        <!-- true: 不被子类自动导入 -->
        <!-- false: 子类必须继承并自动导入此依赖 -->
        <optional>true</optional>
        <!-- 使用范围,对当前项目影响 -->
        <!-- compile,默认值,当前依赖参与编译并打包 -->
        <!-- test,仅参与测试工作和编译,如junit -->
        <!-- runtime,运行时可用 -->
        <!-- provided,参与编译、测试、运行等周期,但不参与打包 -->
        <!-- system,依赖项不从maven仓库取,从本地文件中取,与systemPath配合 -->
        <!-- import,引入其他项目的依赖 -->
        <scope></scope>
        <!-- 当scope为system时,此配置项生效,本地jar路径 -->
        <systemPath></systemPath>
        <!-- 排除此依赖下的jar -->
        <exclusions>
            <exclusion>
                <groupId><groupId>
                <artifactId></artifactId>
            </exclusion>
            <exclusion></exclusion>
            ...
        </exclusions>
    </dependency>
    ...
</dependencies>

```
> [!note]dependency not found
> 注意依赖版本问题和版本兼容性
### build
定义项目构建过程
plugins是必选的,其他的一般是可选的,不需要配置的
```xml
<build>
    <!-- 执行`mvn`时的默认操作,可以简化操作 -->
    <defaultGoal>package</defaultGoal>
    <!-- build的输出目录,默认是target.可以指定如out -->
    <directory>target</directory>
    <!-- 指定生成jar包的文件名 -->
    <!-- 若不指定生成的文件名格式为${artifactId}-${version} -->
    <finalName>myproject</finalName>
    <!-- fileters,过滤不参与build的文件 -->
    <filters>
        <filter>src/main/filters/filter.properties</filter>
    </filters>
    <!-- plugins,重要,一般是定义maven build插件 -->
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <excludes>
                    <exclude>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
    <!-- 资源路径,指定 -->
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
            </includes>
        </resource>
    </resources>
</build>
```
> [!tip] 如当设置defaultGoal为package时,package是加粗显示的
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-img-20241215182054.png)

### distributionManagement
分发构件的配置, 执行`mvn deploy`时发送到配置的远程仓库
Maven会依据版本分发到release或snapshot仓库
```xml
<distributionManagement>
    <!-- release仓库,稳定 -->
    <repository>
        <id>nexus-releases</id>
        <url>http://192.168.120.172:8081/repository/maven-releases/</url>
    </repository>
    <!-- 快照仓库,实时不稳定 -->
    <snapshotRepository>
        <id>nexus-snapshots</id>
        <url>http://192.168.120.172:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>

```
### repository
在pom文件中以\<repositories></repositories>标签配置仓库
```xml
<repositories>
	<repository>
		<id>nexus-public</id>
		<url>http://192.168.120.172:8081/repository/maven-public/</url>
	</repository>
    <!-- 配置多个仓库 -->
	<repository>
		<id>aliyun</id>
		<url>https://maven.aliyun.com/repository/public</url>
	</repository>
</repositories>

```

## 仓库
### 分类
- 本地库 ~/.m2/repository 
- 远程库
    - 私有库(公司搭建的开发库)
    - 公开库(开源仓库,如mvn、ali镜像等仓库)
> [!note]优先级
> 先在本地库中找, 若找不到从远程库中寻找下载到本地库中
### 配置
- [仓库的配置](#repository)
- 仓库认证(访问)的配置
```xml
<!--此处设置的用户名和密码都是nexus的登陆配置-->
<servers>
    <server>
        <!--对应pom.xml的id=releases的仓库-->
        <id>releases</id>  
        <username>xuxiaoxiao</username>
        <password>xuxiaoxiao123</password>
    </server>
    <server>
        <!--对应pom.xml中id=snapshots的仓库-->
        <id>snapshots</id>
        <username>xuxiaoxiao</username>
        <password>xuxiaoxiao123</password>
    </server>
</servers>
```

## Command
`mvn clean` 清除输出目录(一般是target,编译和打包文件)
`mvn validate`
`mvn compile`
`mvn test`用于执行项目的单元测试
```sh
# 单元测试类名为 MyTest.java,如下测试
mvn test -Dtest=MyTest
```

`mvn package`打包,jar/war等
`mvn verify`可以确保项目的构建成功和构建结果的正确性,项目发布前验证
`mvn install`将构件安装到本地仓库
`mvn site`需要引入插件,生成站点文件
`mvn deploy`将构件分发到远程仓库
