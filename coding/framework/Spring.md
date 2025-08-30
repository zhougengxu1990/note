# Spring
## pom
POM是项目对象模型(Project Object Model)的简称,它是Maven项目中的文件，使用XML表示，名称叫做pom.xml。作用类似ant的build.xml文件，功能更强大。该文件用于管理：源代码、配置文件、开发者的信息和角色、问题追踪系统、组织信息、项目授权、项目的url、项目的依赖关系等等。事实上，在Maven世界中，project可以什么都没有，甚至没有代码，但是必须包含pom.xml文件
### pom结构
```xml
<!-- 声明xml文件格式以及utf-8编码 -->
<?xml version="1.0" encoding="UTF-8"?>

<!-- 项目 -->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
         <!-- pom版本 -->
    <modelVersion>4.0.0</modelVersion>

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

    <!-- 当前项目分组 -->
    <groupId>com.example</groupId>
    <!-- 当前项目或模块 -->
    <artifactId>springdemo</artifactId>
    <!-- 当前版本 -->
    <version>0.0.1-SNAPSHOT</version>
    <!-- 项目名 -->
    <name>springdemo</name>
    <!-- 描述 -->
    <description>springdemo</description>
   
    <!-- 所有属性配置 -->
    <properties>
        <!-- 相关配置 -->
        <java.version>17</java.version>
        <spring-cloud.version>2023.0.1</spring-cloud.version>
    </properties>

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
            <!-- runtime -->
            <!-- provided,参与编译、测试、运行等周期,但不参与打包 -->
            <!-- system,依赖项不从maven仓库取,从本地文件中取,与systemPath配合 -->
            <!-- import -->
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

    <!-- 依赖管理 -->
    <!-- 用于项目管理统一版本,子项目引入依赖时不需要指定version,version与父项目保持一致 -->
    <!-- 当此处version变更时,可全局管理和变更版本 -->
    <!-- 当然,子项目在依赖可指定version,不受父项目声明影响 -->
    <!-- 此处只是声明依赖,并不实现自动引入 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>5.1.44</version>
            </dependency>
        </dependencies>
    </dependencyManagement>


    <!-- build项目 -->
    <build>
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
    </build>
</project>
```
