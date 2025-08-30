# JavaDoc
Java 文档注释(Java Doc Comments)是专门为了用javadoc工具自动生成文档而写的注释，文档注释与一般注释的最大区别在于起始符号是/**而不是/*或//。

文档注释只负责描述类(class)、接口(interface)、方法(method)、构造器(constructor)、成员字段(field)。相应地，文档注释必须写在类、接口、方法、构造器、成员字段前面，而写在其他位置，比如函数内部，是无效的文档注释。

文档注释采用HTML语法规则书写，支持HTML标记(tag)，同时也有一些额外的辅助标记。需要注意的是，这些标记不是给人看的（通常他们的可读性也不好），可以通过 Javadoc 命令把文档注释中的内容生成文档，并输出到 HTML 文件中

## tag
JavaDoc默认常用tag
> 写于/**...*/注释中,*与@tag之间不能有其他字符,否则失效
```javadoc
/**
 * @author 作者
 * @since  从哪个版本开始生效
 * @version 版本
 * @param 参数
 * @return 返回值说明
 * @throws 抛出什么异常
 * @see 引用相关内容,如类、方法、变量等
 */
```
内联tag
> {@tag} 格式的标签
- `{@link path label}`相当于锚定一个类、方法或变量位置,并用label描述说明
```javadoc
/**
 * 1.使用"#"link当前类中的变量或方法(包括构造方法)(可以link自己,但无意义,会提示warning)
 * {@link #ok 当前类中的变量}
 * {@link #test(int) 当前类中的方法}
 *
 * 2.link其他类
 * {@link com.zgx.demo.service.TestService 其他类}
 *
 * 3.link其他类#方法(或变量)
 * {@link com.zgx.demo.service.TestService#t(int, String)}  其他类}
 *
 * 4.label会替换引用path作为描述
 */
```


## template
类、接口或枚举注释模板
```javadoc
/**
 * ${todo}
 * @author ${USER}
 * @Date ${YEAR}-${MONTH}-${DAY} ${TIME}
 * @version 1.0
 */
```
- `@author` `@version`等是JavaDoc自带tag,`@Date`是自定义tag,会被warning提示
- 对于非内置参数,如`${todo}`会有手动输入的步骤
![Img](https://raw.githubusercontent.com/zhougengxu1990/picture-go/master/yank-note-picgo-%E6%88%AA%E5%B1%8F2024-05-07%2006.33.22.png =500x)
```java
/**
 * 测试注释
 *
 * @author zhougengxu
 * @version 1.0
 * @Date 2024-05-07 06:32
 */
```
## 自定义user
```
//虚拟机中
-Duser.name=author
```
