# Java基础
## 初识
> 一种高级开发语言
### 命名规范
- 禁止使用中文全拼
- 不能以数字开头
- 只能以字母开头,不建议使用下划线
- 不宜过长,但需要表达完整的意义的英文
- 包名:域名后缀.域名.项目名(全英文小写,用"."分隔)
- 驼峰规则
    - 类名:大驼峰
    - 方法名:小驼峰,一般用动词
    - 变量:小驼峰
- 常量/枚举:大驼峰

<br/>

### 特性
- 一次开发,到处使用
- 开源,生态完善
- 纯粹的面向对象开发语言

<br/>

### 开发方向
- 服务器开发
- 移动开发
- 桌面开发
- 大数据/人工智能
- JavaSE
- JavaEE
- JavaMe
- JDK
- JRE
- JVM

<br/>

### 类、变量和方法

```java
//包名
package com.zgx.demo;

//类
public class Test {
    //类变量
    private int a = 1;

    //方法(无参)
    public int getA() {
        //用return返回值
        return a;
    }

    //方法(有参)
    //void修饰表示方法没有返回值
    public void setA(int a) {
        //this指此对象.this.a指的是对象的变量a
        //等号后面的a,由于就近原则,指的传参a
        this.a = a;
    }

    //main 主方法
    public static void main(String[] args) {

    }
}

```

<br/>

## 数据类型
### 分类
- 基本数据类型(原生数据类型)
- 引用数据类型

<br/>

### 基本数据类型
| 数据类型 | 名称 | 占用字节 | 范围 | 默认值 |
| -- | -- | -- | -- | -- |
| byte | 字节 | 1 | -128 ~ 127 | 0b0 |
| short | 短整形 | 2 | -2^15 ~ 2^15-1 | 0 |
| int | 整形 | 4 | -2^31 ~ 2^31-1 | 0 |
| long | 长整形 | 8 | -2^63 ~ 2^63-1 | 0 |
| float | 单浮点数 | 4 | 32位，正负1位，指数位8位，小数位23位，只能保证运算小数位6位的精度 | 0.0f |
| double | 双浮点数 | 8 | 64位，正负1位，指数位11位，小数位52位，能保证运算小数位15位精度 | 0.0d |
| char | 字符 | 2 | 0 ~ 65535 | \u0000(空格) |
| boolean | 布尔 | 1bit | true/false | false |

### 引用数据类型
除了基本数据类型,Java内置的或自定义的数据类型都是引用数据类型,又称包装类型
```java
        //基本数据类型
        byte b = 1;
        short s = 2;
        int i = 3;
        long l = 4L;//long类型最好以'L'结尾
        float f = 5.1f;//float类型最好以'f'结尾
        double d = 6.2d;//double类型最好以'd'结尾
        char c = 'c';
        boolean bool = false;

        //引用数据类型
        Integer ii= 1;
        Long ll = 2L;
        Float ff = 3.2f;
        Double dd = 4.1d;
        Boolean bb = true;
```

<br/>

## 面向过程
### 顺序
顺序向下就是顺序
```java
int a = 1;
System.out.println(a);
```

### 分支
因为条件判断,就出现了分支逻辑
- if
```java
   //if...
   if(){
            
    }
    
    //if...else...
    if(){
        
    }else{
        
    }
    
    //if...else if...
    if(){
        
    }else if(){
        
    }
    
    //if...else if...else...
    if(){
        
    }else if(){
        
    }else{
        
    }
```
- switch...case...
```java
        switch (i){
            //只有i符合case的条件的执行,并用break;跳过其他的case则不再检测.注意:一定要注意break;
            case 1:
                break;
            case 2:
                break;
            //可以用{}写方法体
            case 3:{
                //...

                break;
            }
            //case都不符合,则执行default
            default:
                break;
        }
```

### 循环
- for
```java
    ArrayList<Object> list = new ArrayList<>();
    //foreach
    for (Object o : list) {
        //...

        //使用continue,则continue后面的代码不再执行,进入下一循环
        if(o == null){
            continue;
        }
    }

    //fori
    for (int i1 = 0; i1 < list.size(); i1++) {
        //...

        //使用break;可以跳出循环
        if(list.get(i1) == null){
            break;
        }
    }
    
    //特殊,不停止的循环,等价于while(true)
    for (;;){
        
    }

```

- while
```java
    while(条件){
        //...
        //只要条件符合,则循环不止
        //可以改变条件中的值,使得条件=false则循环停止
        //或直接使用break结束循环
    }
    
```

<br/>

## 面向对象
### OO
> Object Oriented

| 缩写 | 描述 |
| -- | -- |
| OOL  | Object Oriented Language    |
| OOD  | Object Oriented Design      |
| OOP  | Object Oriented Programming |
| OOA  | Object Oriented Analysis    |
## String