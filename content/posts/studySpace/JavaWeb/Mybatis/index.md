---
date: '2025-06-29T19:43:52+08:00'
draft: false
title: 'Mybatis'
seriesOpened: true #s是否开启系列
series: ["JavaWeb"] #属于的系列 
series_order: 4  #系列编号
showSummary: true #是否显示摘要
summary: "" #摘要信息
tags: ["Web"]
Categories: ["Java"]
layoutBackgroundBlur: true #向下滚动主页时，是否模糊背景图。
layoutBackgroundHeaderSpace: true #在标题和正文之间添加空白区域间隔。
---



## JDBC

测试 JDBC 代码

~~~java
public class JDBCTest {gfrv

    /**
     * 编写JDBC程序, 查询数据
     */
    @ParameterizedTest
    @CsvSource({"daqiao,123456"})
    public void testJdbc(String _username, String _password) throws Exception {
        // 获取连接
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/web", "root", "1234");
        // 创建预编译的PreparedStatement对象
        PreparedStatement pstmt = conn.prepareStatement("SELECT * FROM user WHERE username = ? AND password = ?");
        // 设置参数
        pstmt.setString(1, _username); // 第一个问号对应的参数
        pstmt.setString(2, _password); // 第二个问号对应的参数
        // 执行查询
        ResultSet rs = pstmt.executeQuery();
        // 处理结果集
        while (rs.next()) {
            int id = rs.getInt("id");
            String uName = rs.getString("username");
            String pwd = rs.getString("password");
            String name = rs.getString("name");
            int age = rs.getInt("age");

            System.out.println("ID: " + id + ", Username: " + uName + ", Password: " + pwd + ", Name: " + name + ", Age: " + age);
        }
        // 关闭资源
        rs.close();
        pstmt.close();
        conn.close();
    }

}
~~~

## Mybatis

**简介**

MyBatis 是一款优秀的持久层框架，它支持自定义 SQL、存储过程以及高级映射。MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

中文文档 https://mybatis.org/mybatis-3/zh/index.html


**导入依赖**
~~~xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.11</version>
</dependency>
~~~

**配置**  
在 `application.yml` 文件中进行配置
~~~yml
# 数据源配置
spring:
  datasource:
    # 数据库连接URL
    url: jdbc:mysql://localhost:3306/web
    # JDBC驱动类全名(MySQL 8.0+使用cj驱动)
    driver-class-name: com.mysql.cj.jdbc.Driver
    # 数据库用户名
    username: root
    # 数据库密码
    password: root@1234

# MyBatis配置
mybatis:
  configuration:
    # 启用控制台SQL日志输出
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    # 自动将下划线命名转为驼峰命名
    map-underscore-to-camel-case: true
~~~

Mapper层中编写代码
~~~java
@Mapper
public interface UserMapper {
    @Select("select * from user")
    public List<User> list();
}
~~~

### 数据库连接池

数据库连接池是个容器，负责分配、管理数据库连接，在程序在启动时，会在数据库连接池(容器)中，创建一定数量的Connection对象，允许应用程序重复使用一个现有的数据库连接，

#### 配置连接池

**导入依赖**
~~~xml
<dependency>
    <!-- Druid连接池依赖 -->
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.19</version>
</dependency>
~~~

**配置驱动**：在 `application.properties` 加入配置
~~~properties
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
~~~

## 执行方法
建立一个 `DemoMapper` 接口，使用 `@Mapper` 标注。

通过再接口方法上使用 `@select` 、 `@insert` 、 `@updata` 、 `@delete` 标注，值为执行的sql语句

## 参数处理
sql 语句中要使用 `#{itemName}` 进行占位，

当只有一个参数时，参数名和占位的名称可以不一致，如果为多个参数要使用 `@Param` 设置参数引用的名字，

如果使用 xml 动态配置 sql 则必须使用 `@Param` 设置参数引用的名字，

## 动态 SQL

在 resources 文件夹下创建目录， xml 配置文件名要和 `DemoMapper` 接口名保持一致，文件目录结构要和引用保持一致，比如一个引用路径为 `cn.it.demo.mapper` 那么目录结构为 `cn/it/demo/mapper` 

### 基本结构

~~~xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

        <!-- namespace：绑定的mapper接口 -->
<mapper namespace="cn.it.demo.mapper.modelName">

<!-- id：绑定接口中的方法 -->
    <select id="Demo_Mapper_functionName" resultType="cn.it.demo.model.ModelName">
        select *
        from TableName
        group by job
    </select>
</mapper>
~~~

可以使用 `#{}` 取出方法参数值，但是方法参数值需要使用 `@Param` 定义别名

### 基础标签
- `select` 查询操作
- `insert` 插入操作
- `updata` 更新操作
- `delete` 删除操作 

### 动态标签

1. `<if>` - 条件判断
   **作用**：根据条件包含 SQL 片段。
    ~~~xml
    <if test="name != null">
        AND name = #{name}
    </if>
    ~~~
2. <choose>/<when>/<otherwise> 多路选择
   **作用**：选择第一个满足的条件
    ~~~xml
    <where>
        <choose>
            <when test="name != null">
                AND name = #{name}
            </when>
            <when test="email != null">
                AND email = #{email}
            </when>
            <otherwise>
                AND status = 'ACTIVE'
            </otherwise>
        </choose>
    </where>
    ~~~
3. <where> 智能 WHERE 子句
    **作用**：自动去除开头的 AND 或 OR
    ~~~xml
    <where>
        <if test="name != null">
            name = #{name}
        </if>
        <if test="age != null">
            AND age = #{age}
        </if>
    </where>
    ~~~
4. <set> 智能 SET 子句
    **作用**：用于更新语句，自动去除结尾的逗号。
    ~~~xml
    <update id="updateUser">
        UPDATE user
        <set>
            <if test="name != null">name = #{name},</if>
            <if test="age != null">age = #{age},</if>
        </set>
        WHERE id = #{id}
    </update>
    ~~~
5. <foreach> 循环遍历
    **作用**：遍历集合
    ~~~xml
    <select id="findUsersByIds" resultType="User">
        SELECT * FROM user
        WHERE id IN
        <foreach collection="ids" item="id" open="(" close=")" separator=",">
            #{id}
        </foreach>
    </select>
    ~~~
    collection：参数名
    item：元素名
    open：开始符号
    close：结尾符号
    separator：分隔符
## 映射
**映射类型**
1. 自动映射（Auto-Mapping）
    ~~~xml
    <select id="selectUser" resultType="User">
        SELECT * FROM user WHERE id = #{id}
    </select>
    ~~~
    特点：根据列名自动匹配对象属性

    规则：列名 → 属性名（如 user_name → userName）

    *需配置 `mapUnderscoreToCamelCase=true` 开启驼峰转换*

2. 显式映射（ResultMap）
    ~~~xml
    <resultMap id="userMap" type="User">
        <id property="id" column="user_id"/>
        <result property="username" column="user_name"/>
    </resultMap>
    ~~~
    优势：可处理复杂映射关系

### 基础映射
~~~xml
<resultMap>
    <!-- 主键映射 -->
    <id property="java属性" column="数据库列"/>
    
    <!-- 普通字段映射 -->
    <result property="java属性" column="数据库列"/>
</resultMap>
~~~

### 关联映射（1对1）
~~~xml
<association property="dept" javaType="Department">
    <id property="id" column="dept_id"/>
    <result property="name" column="dept_name"/>
</association>
~~~

### 集合映射（1对多）
~~~xml
<resultMap id="resultMap_id" type="cn.it.demo.model.modelName_1" autoMapping="true">
    <id property="model_1_Item" column="table_1Column"></id>
    <collection property="exprList" ofType="cn.itcast.pojo.modelName_2" autoMapping="true" column="table_1Column">
        <id column="model_2_Item" property="table_2Column"></id>
        <result property="job" column="exprJob"></result>
    </collection>
</resultMap>
~~~

### 多路映射（Discriminator）
~~~xml
<discriminator javaType="String" column="user_type">
    <case value="1" resultMap="adminMap"/>
    <case value="2" resultMap="vipMap"/>
</discriminator>
~~~

## 分页

引入依赖 PageHelper
~~~xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.4.7</version>
</dependency>
~~~

~~~java
public Page<Emp> findByConditionAndPage(Integer page, Integer pageSize) {
    //开启分页助手
    PageHelper.startPage(page, pageSize);
    List<Emp> byConditionAndPage = empMapper.findByConditionAndPage();
    log.warn("采用分页助手的我们的返回值已经被篡改,已经是list的子类, 里面还有很多东西{}", byConditionAndPage);
    Page<Emp> p = (Page<Emp>) byConditionAndPage;
    // p.getTotal()  //获取数据总条数
    // p.getResult() //获取当前页的集合
    return p;
}
~~~

## 一对多映射

需要手动配置映射关系，讲 `resultType` 换为 `resultMap`

resultMap