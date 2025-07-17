---
date: '2025-06-29T14:37:43+08:00'
draft: false
title: 'Maven、Junit-(单元测试)'
seriesOpened: true #s是否开启系列
series: ["JavaWeb"] #属于的系列 
series_order: 1  #系列编号
showSummary: true #是否显示摘要
summary: "" #摘要信息
tags: ["Maven"]
Categories: ["Java"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

Maven是一个基于Java的构建工具，可以帮助开发者快速构建、管理和发布Java项目。它利用了项目对象模型（Project Object Model，POM）来描述项目构建的过程，主要作用有 依赖管理，项目构建，统一项目结构。

包管理文件 `pom`

基础结构
~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>  <!-- 组织标识 -->
    <artifactId>a</artifactId>      <!-- 项目标识 -->
    <version>1.0-SNAPSHOT</version> <!-- 项目标识 -->

    <dependencies>  <!-- 依赖列表 -->
        <!--导入jackson-->
        <dependency> 
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.15.2</version>
        </dependency>
    </dependencies>
</project>
~~~

## 依赖管理
### 导入依赖
1. 可以将 jar 放入本地的 maven 仓库中，
2. 在pom.xml中编写<dependencies>标签
3. 在<dependencies>标签中使用<dependency>引入坐标
4. 定义坐标的 `groupId` 、 `artifactId` 、 `version`

### 查找依赖
`alt` + `insert` 

### 依赖传递
当我们导入一个jar的坐标的时候,如果这个。
jar依赖了其他jar包,那么其他依赖jar也会一并导入。

### 依赖冲突
1. 如果两个jar包的层级一样，后面的会将前面的覆盖。
2. 如果层级一样，层级越浅的优先级越高。
3. 如果不同的jar内部的依赖冲突了, 那么我们谁先声明谁优先

### 依赖排除
~~~xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>6.1.4</version>

    <!--排除依赖, 主动断开依赖的资源-->
    <exclusions>
        <exclusion>
            <groupId>io.micrometer</groupId>
            <artifactId>micrometer-observation</artifactId>
        </exclusion>
    </exclusions>
</dependency>
~~~

### 依赖范围

可以通过 `<scope>…</scope>` 设置其作用范围。


| scope值         | 主程序 | 测试程序 | 打包（运行） |
| --------------- | ------ | -------- | ------------ |
| compile（默认） | Y      | Y        | Y            |
| test            | -      | Y        | -            |
| provided        | Y      | Y        | -            |
| runtime         | -      | Y        | Y            |

### 生命周期
对所有的构建过程进行抽象和统一。 描述了一次项目构建，经历哪些阶段。

生命周期包含了项目的清理，初始化，编译，测试，打包，集成测试，验证，部署和站点生成等几乎所有构建步骤。

Maven对项目构建的生命周期划分为3套：
1. clean：清理工作。
2. default：核心工作。如：编译、测试、打包、安装、部署等。
3. site：生成报告、发布站点等。

## 单元测试 Junit

### 导入依赖 

~~~xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.9.1</version>
    <scope>test</scope>
</dependency>
~~~

在 text/java目录下，创建测试类，并写相应的测试代码

{{<alert>}}
- 测试类的命名规范为：XxxxTest
- 测试方法的命名规定为：public void xxx(){...}
{{</alert>}}

在测试方法上加上 `@Test` 注解，就可以开始测试了，

一些常用的测试注解

| 注解               | 说明                                                             |
| ------------------ | ---------------------------------------------------------------- |
| @Test              | 测试类中的方法用它修饰才能成为测试方法，才能启动执行             |
| @BeforeEach        | 用来修饰一个实例方法，该方法会在每一个测试方法执行之前执行一次。 |
| @AfterEach         | 用来修饰一个实例方法，该方法会在每一个测试方法执行之后执行一次。 |
| @BeforeAll         | 用来修饰一个静态方法，该方法会在所有测试方法之前只执行一次。     |
| @AfterAll          | 用来修饰一个静态方法，该方法会在所有测试方法之后只执行一次。     |
| @ParameterizedTest | 参数化测试的注解 (可以让单个测试运行多次，每次运行时仅参数不同)  |
| @ValueSource       | 参数化测试的参数来源，赋予测试方法参数                           |
| @DisplayName       | 指定测试类、测试方法显示的名称 （默认为类名、方法名）            |

### 断言

| 断言方法                                              | 描述                                       |
| ----------------------------------------------------- | ------------------------------------------ |
| assertEquals(Object exp, Object act, String msg)      | 检查两个值是否相等，不相等就报错。         |
| assertNotEquals(Object unexp, Object act, String msg) | 检查两个值是否不相等，相等就报错。         |
| assertNull(Object act, String msg)                    | 检查对象是否为null，不为null，就报错。     |
| assertNotNull(Object act, String msg)                 | 检查对象是否不为null，为null，就报错。     |
| assertTrue(boolean condition, String msg)             | 检查条件是否为true，不为true，就报错。     |
| assertFalse(boolean condition, String msg)            | 检查条件是否为false，不为false，就报错。   |
| assertSame(Object exp, Object act, String msg)        | 检查两个对象引用是否相等，不相等，就报错。 |