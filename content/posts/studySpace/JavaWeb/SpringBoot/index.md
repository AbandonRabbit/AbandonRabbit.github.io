---
date: '2025-06-29T18:07:22+08:00'
draft: false
title: 'SpringBoot'
seriesOpened: true #s是否开启系列
series: ["JavaWeb"] #属于的系列 
series_order: 3  #系列编号
showSummary: true #是否显示摘要
summary: "" #摘要信息
tags: ["SpringBoot"]
Categories: ["Java"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---

是一个基于 Spring 框架的开源 Java 开发框架，旨在简化 Spring 应用的初始搭建和开发流程。它通过约定优于配置的原则和自动化配置机制，让开发者能够快速构建独立、生产级的 Spring 应用程序。

引入依赖
~~~xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.8.27</version>
</dependency>
~~~

**目录结构**
- pojo： 放实体类
- controller：控制器
- exception： 异常
- mapper：数据访问层
- utils：工具类
- service：服务层
  - impl：服务层的代码
  - 各个服务接口：impl中的类实现响应的接口。
- config：配置
- filter：拦截器
- interceptor：过滤器

**基本开发流程**

在 controller 中声明一个 controller 类加上控制器相关的注解，再写 service 层，最后写数据库访问层。

**springboot 配置文件**

在模块下的 application 文件中进行配置，需要注意以下优先级

命令行参数 > jvm参数 > properties> yml > yaml

取出配置文件中的值使用 `@Value()` 注解。

## 创建服务

### 用到的注解

`@Controller` ： 将一个类标记为 Web 请求的控制器组件。

`@ResponseBody` ：将方法返回值直接响应给浏览器，如果返回值类型是实体对象/集合，将会转换为JSON格式后在响应给浏览器，书写在Controller方法上或类上。

`@RequestMapping` ：设置访问的路由，用来将 HTTP 请求映射到 MVC 控制器的处理方法上。

`@RestController` ：这个注解是由两个注解组合起来的，分别是：`@Controller` 、`@ResponseBody`。

### 处理请求

**处理方法上的注解**

`@RequestMapping` ：
- 作用：定义请求映射路径，支持类/方法级别
- 属性：value（路径）、method（请求方法）、produces（响应类型）

RequestMapping 下有一些衍生注解：`@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`。

**路径参数**  
指直接嵌入在 URL 路径中的变量值，如 `https://api.example.com/{id}`，其中 id 就是路径参数

### 参数绑定

**绑定路径参数**

如 `get` 请求中的路径参数：在处理方法上加上注解 `@GetMapping("/{id}")` 用来接收处理方法，需要注意方法中的参数名要保持一致。

**URL 请求参数**

在 URL 中以 `?key=value&key=value` 形式的参数。

需要使用 `@RequestParam(键)` 注解进行绑定

> `@RequestParam` 主要属性  
> `value` / `name` 指定参数名，默认使用方法参数名
> `required` 是否为必须
> `defaultValue` 默认值

ps: 如果参数很多，可以直接将方法参数类型定义为 `Map<String,String>` 所有参数都会放在这个map中。

ps：如果有日期参数，还需要在方法参数加上 `@DateTimeFormat(pattern = "yyyy-MM-dd")` 注解。

**请求体参数**

使用 `@RequestBody` 注解：绑定到对象上，对象内部的成员属性需要为引用类型

时间类型需要使用 `@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")` 标注。

### 图片上传

需要使用 `MultipartFile` 类型的参数接收数据，必须使用 Post 方式提交

`MultipartFile` 对象常用的方法

| 方法名                        | 描述                       |
| ----------------------------- | -------------------------- |
| String getOriginalFilename(); | 获取原始的名字             |
| String getName()              | 提交的key的名字            |
| InputStream getInputStream(); | 获取输入流读取上传文件     |
| void transferTo(目的地);      | 转存文件，目的地为File对象 |

**多文件管理方案**

根据文件名计算hash值，并根据hash值建立多级目录。根据目录可以快速索引到文件。

## 全局异常处理

创建自定义异常类继承 `RuntimeException` 异常

定义一个类，使用 `@RestControllerAdvice` 标注，这个类就是全局异常处理的类

在全局异常类中创建异常处理方法，参数为自定义异常类，方法使用 `@ExceptionHandler` 标注，值为要处理的异常的字节码（要和方法参数一致）。

返回值的数据会直接放到响应中。

## 过滤器

需要在启动类加上注解 `@ServletComponentScan` 开启 servlet 支持

执行在 DispatcherServlet 之前，用来对访问数据的过滤，

**多个过滤器执行顺序**：默认按照名字的字典顺序排序执行，在调用doFilter后会按顺序入栈，等到执行完之后在依次出栈。

过滤器中的异常不能被 spring 框架中注册的全局异常抓到

### 定义过滤器

定义一个类实现 `Filter` 重写所有的方法，使用 `@WebFilter` 注解配置拦截的请求路径，`@WebFilter(urlPatterns = "/*")`  表示拦截浏览器的所有请求

### Filter 中的方法

| 方法名   | 描述                                                          |
| -------- | ------------------------------------------------------------- |
| init     | 初始化方法, web服务器启动, 创建Filter实例时调用, 只调用一次。 |
| doFilter | 拦截到请求时,调用该方法,可以调用多次。                        |
| destroy  | 销毁方法, web服务器关闭时调用, 只调用一次。                   |

如果执行了一个操作不放行，就无法执行后面的操作。


## 拦截器

由 Spring 框架提供，执行在 Controller 之前。

**多个过滤器执行顺序**： `preHandle` 按照注册的顺序执行， `postHandle` 和 `afterCompletion` 方法反之，如果一个拦截器失败，所有的 `postHandle` 都不会再走

### 定义拦截器

拦截器是Spring框架提供的，用来动态拦截控制器方法的执行。

实现HandlerInterceptor接口，并重写其所有方法，并使用 `@Component` 注解标注。

新建一个 `WebConfig` 类，实现 `WebMvcConfigurer` 接口，并重写 `addInterceptors` 方法

~~~java
@Configuration  
public class WebConfig implements WebMvcConfigurer {

    //自定义的拦截器对象
    @Autowired
    private DemoInterceptor demoInterceptor;

    
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
       //注册自定义拦截器对象
        registry.addInterceptor(demoInterceptor).addPathPatterns("/**");//设置拦截器拦截的请求路径（ /** 表示拦截所有请求）
    }
}
~~~

`HandlerInterceptor` 中的方法

| 方法名          | 描述                                                           |
| --------------- | -------------------------------------------------------------- |
| preHandle       | 目标资源方法执行前执行。 返回true：放行    返回false：不放行。 |
| postHandle      | 目标资源方法执行后执行。                                       |
| afterCompletion | 视图渲染完毕后执行，最后执行。                                 |