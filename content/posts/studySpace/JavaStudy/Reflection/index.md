---
date: '2025-06-14T18:22:35+08:00'
draft: false
title: '反射'
seriesOpened: true #s是否开启系列
series: ["java学习笔记"] #属于的系列 
series_order: 13  #系列编号
showSummary: true #是否显示摘要
summary: "" #摘要信息
tags: ["Java基础"]
Categories: ["Java","学习笔记"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

在运行时动态获取类的信息以及动态调用对象方法的能力。反射允许程序在运行时检查和修改它本身的结构和行为，特别是类的属性、方法和构造函数等。

## 反射基本操作

### 获取字节码对象

~~~java

类名.class;

对象.getClass();

Class.forName(全类名);  //全类名是指 包名.类名
~~~

### 获取构造函数

~~~java
//获取所有的public修饰的构造函数们
Constructor[]  getConstructors(); 
//获取单个public修饰的构造 (常用!!!!!!!!!!!)
Constructor  getConstructor(Class... param); //参数为构造函数的参数列表
//获取所有的修饰的构造函数们
Constructor[]  getDeclaredConstructors();   
//获取单个构造(常用!!!!!!!!!!!)
Constructor  getDeclaredConstructor(Class... param); //参数为构造函数的参数列表

//调用构造函数
constructor.newInstance(Class... param) 
~~~

如果访问私有方法或成员变量需要使用 `setAccessible(true);` 。

### 获取成员变量

~~~java
// 获取所有public修饰的成员变量
Field[]  getFields(); 
// 获取名字为name的public修饰的成员方法
Filed   getField(String name); 
// 获取所有成员变量
Field[]  getDeclaredFields(); 
// 获取单个成员变量
Field   getDeclaredField(String name);

//成员变量 set 操作
field.set(object,value);
//成员变量 get 操作
ffield.get(object);
~~~

### 获取成员方法

~~~java

// 获取所有public修饰的方法,包含父类的方法
Method[] getMethods(); 
// 获取所有本类的方法,不能获取父类的方法
Method[] getDeclaredMethods(); 
// 获取单个的方法, 包含父类的public修饰的方法
Method getMethod(String name,Class...args); 
// 获取本类的单个方法
Method getDeclaredMethod(String name,Class...args); 

//调用
Object result = method.invoke(object, args...)
~~~

### 静态成员变量和方法

获取和设置没差别，只需要把 object 替换为 null 就可以了。

**Properties 类**

属性集，专用用于处理字符串的键和值，底层采用 Map 存储。

*常用方法*
| 方法名                                           | 描述               |
| ------------------------------------------------ | ------------------ |
| `load(字节/字符输入流)`                          | 加载               |
| `String  setProperty(String key, String value);` | 添加键值对         |
| `String  getProperty(String key);`               | 根据 k 获取 v      |
| `Set<String> stringPropertyNames();`             | 获取 k 的 Set 集合 |


## 动态代理

在运行时动态生成，生成的代理类继承 `Proxy` 类，并实现指定的接口，通过反射调用真实对象的方法。所有的方法调用都通过 `invoke()` 方法路由。

### 创建代理对象
~~~java
Subject proxy = (Subject) Proxy.newProxyInstance(
    Subject.class.getClassLoader(), // 类加载器
    new Class[]{Subject.class},     // 代理接口数组
    new MyInvocationHandler(real)   // InvocationHandler实现
);
~~~

**动态代理的特点**
- 运行时生成：代理类是在运行时动态生成的。
- 基于接口：只能代理接口，不能代理类（这是与CGLIB的主要区别）。
- 反射调用：通过反射机制调用目标方法。
- 统一处理：所有方法调用都通过invoke()方法路由。

**动态代理的实现原理**
>在运行时动态生成代理类的字节码，生成的代理类继承Proxy类并实现指定的接口，代理类中的每个方法都会调用InvocationHandler的invoke()方法，通过反射调用真实对象的方法。

## 类加载器

类加载器负责在运行时动态将类生成为 `class` 文件，并加载到方法区。

### 层次结构

**双亲委派模型**

当一个类加载器收到类加载请求时，首先不会自己尝试加载，而是委托给父类加载器去完成。只有当父类加载器反馈无法完成加载请求时（在自己的搜索范围内没找到该类），子加载器才会尝试自己加载。这种机制从下到上，再从上到下进行加载。这样加载避免了类重复加载，并保证核心类库的安全。

1. Bootstrap ClassLoader（启动类加载器）
   最顶层的类加载器，主要用来加载 java 的核心类库。
2. Extension ClassLoader（扩展类加载器）
   负责加载jdk的扩展，会将jdk下ext中的类加载。
3. Application ClassLoader（应用程序类加载器/系统类加载器）
   用来加载除了jdk之外的类，可以通过 `ClassLoader.getSystemClassLoader()` 获取。
4. 自定义类加载器
   可以继承 ClassLoader 类实现自己的类加载器。

**动态的获取字节码路径**

~~~java
ClassLoader c1 = ClassLoader.getSystemClassLoader();
// InputStream in = c1.getResourceAsStream("fruit.properties");
URL url = c1.getResource("fruit.properties");
String path = url.getPath();
System.out.println(path);
~~~

