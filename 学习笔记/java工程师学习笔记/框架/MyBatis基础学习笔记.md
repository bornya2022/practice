# MyBatis基础学习笔记

## 一.MyBatis简介

### 1.1 ORM

首先，我们来了解一下什么是 ORM ？

ORM（Object/Relational Mapping），即对象关系映射，它完成面向对象的编程语言到关系数据库的映射。ORM 工具的唯一作用是：把持久化对象的保存、修改、删除等操作，转换成对数据库的操作。

ORM 基本映射关系：

- 数据表映射类
- 数据表的行映射对象（实例）
- 数据表的列（字段）映射对象的属性

MyBatis 本是 apache 的一个开源项目 iBatis , 2010 年这个项目由 apache software foundation 迁移到了 google code，并且改名为 MyBatis 。

MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及对结果集的检索封装。`MyBatis 可以对配置和原生 Map 使用简单的 XML 或注解，将接口和 Java 的 POJO(Plain Old Java Objects, 普通的 Java 对象）映射成数据库中的记录。`

MyBatis 的主要思想是将程序中的大量 SQL 语句抽取出来，配置在配置文件中，以实现 SQL 的灵活配置。

MyBatis 并不完全是一种 ORM 框架，它的设计思想和 ORM 相似，只是它允许直接编写 SQL 语句，使得数据库访问更加灵活。因此，准确地说，MyBatis 提供了一种“半自动化”的 ORM 实现，是一种 "SQL Mapping" 框架。

### 1.2 mybatis的功能架构

MyBatis 的功能架构分为三层：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid970timestamp1490850885122.png/wm)

- **API 接口层**：提供给外部使用的接口 API，开发人员通过这些本地 API 来操纵数据库。接口层一接收到调用请求就会调用数据处理层来完成具体的数据处理。
- **数据处理层**：负责具体的 SQL 查找、SQL 解析、SQL 执行和执行结果映射处理等。它主要的目的是根据调用的请求完成一次数据库操作。
- **基础支撑层**：负责最基础的功能支撑，包括连接管理、事务管理、配置加载和缓存处理，这些都是共用的东西，将他们抽取出来作为最基础的组件，为上层的数据处理层提供最基础的支撑。

### 1.3 mybatis的框架架构

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid970timestamp1490851472249.png/wm)

- **加载配置**：MyBatis 应用程序根据 XML 配置文件加载运行环境，创建 SqlSessionFactory，SqlSessionFactory，配置来源于两个地方，一处是配置文件，一处是 Java 代码的注解，将 SQL 的配置信息加载成为一个个 MappedStatement 对象（包括了传入参数映射配置、执行的 SQL 语句、结果映射配置），存储在内存中。
- **SQL 解析**：当 API 接口层接收到调用请求时，会接收到传入 SQL 的 ID 和传入对象（可以是 Map、JavaBean 或者基本数据类型），Mybatis 会根据 SQL 的 ID 找到对应的 MappedStatement，然后根据传入参数对象对 MappedStatement 进行解析，解析后可以得到最终要执行的 SQL 语句和参数。
- **SQL 执行**：SqlSession 将最终得到的 SQL 和参数拿到数据库进行执行，得到操作数据库的结果。
- **结果映射**：将操作数据库的结果按照映射的配置进行转换，可以转换成 HashMap、JavaBean 或者基本数据类型，并将最终结果返回，用完之后关闭 SqlSession。

#### 2.3.1 SqlSessionFactory

每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心的。SqlSessionFactory 是单个数据库映射关系经过编译后的内存映像。SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得。而 SqlSessionFactoryBuilder 则可以从 XML 配置文件或一个预先定制的 Configuration 的实例构建出 SqlSessionFactory 的实例。SqlSessionFactory 是创建 SqlSession 的工厂。

#### 2.3.2 SqlSession

SqlSession 是执行持久化操作的对象，它完全包含了面向数据库执行 SQL 命令所需的所有方法，可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。在使用完 SqlSession 后我们应该使用 finally 块来确保关闭它。

**学习链接，参考资料：**

- [Mybatis 原理分析之二：框架整体设计](http://www.cnblogs.com/jeffen/p/6249007.html)
- [【持久化框架】Mybatis 简介与原理](http://blog.csdn.net/jiuqiyuliang/article/details/45286191)
- [Mybatis 百度百科](http://baike.baidu.com/link?url=aKhaj0KLAyiTQWgJb-7cam6PkPYmDwn1kK0504b7Jbb8LE6MC7npuNhTa-2cmSlKwPCQgM9u--vOSNzvDBrIna)
- [mybatis 学习笔记 (2)-mybatis 概述](https://github.com/brianway/springmvc-mybatis-learning)
- 《Spring+MyBatis 企业应用实战》

## 二.入门实例

### 2.1项目结构文件

![æ­¤å¤è¾å¥å¾ççæè¿°](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542090761913.png/wm)

### 2.2数据库准备

本次课程使用 MySQL 数据库。首先启动 mysql ：

```
$ sudo service mysql start
```

然后在终端下输入以下命令，进入到 MySQL 数据库（-u 表示用户名，比如这里的 root，-p 表示密码，这里没有密码就省略了）：

```
$ mysql -u root
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542077841028.png/wm)

为了实验方便，我们在这里新建一个数据库并取名 `mybatis` 用作实验。

```
create database mybatis;
show databases;
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542077901510.png/wm)

创建表 `user` ，代码如下：

```
use mybatis;
create table user(
id int primary key auto_increment,
username varchar(20),
password varchar(20),
sex varchar(10),
address varchar(20));
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542078002350.png/wm)

### 2.3创建项目

首先打开 WEB IDE，选择 File->Open New Terminal，在终端中输入：

```shell
mvn archetype:generate -DgroupId=com.shiyanlou.mybatis -DartifactId=MyBatisTest -DarchetypeArtifactId=maven-archetype-quickstart
```

参数说明：

- Group Id：项目的组织机构，也是包的目录结构，一般都是域名的倒序，比如 `com.shiyanlou.mybatis`；
- DartifactId ：项目实际的名字，比如 `MyBatisTest`；
- archetype Artifact Id ：使用的 maven 骨架名称

输入命令之后，maven 会提示我们输入版本号，这里可以直接定义版本号也可以直接回车，接着 maven 会提示当前要创建项目的基本信息，输入 y 然后回车确认。 接着切换工作空间到`/home/project/MybatisTest`目录。

### 2.4导入jar包

maven 下载需要联网，这里直接将 maven 下载下来，放到本地仓库便可以解决联网的问题

```
wget http://labfile.oss.aliyuncs.com/courses/802/m2.zip
unzip m2.zip
mv .m2 /home/shiyanlou/
```

打开 pom.xml 文件，修改为以下内容

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.shiyanlou.mybatis</groupId>
    <artifactId>MybatisTest</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>MybatisTest</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.6</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.6</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>

    </dependencies>

    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.*</include>
                </includes>
            </resource>
        </resources>
    </build>
</project>
```

### 2.5配置文件mybatis.cfg.xml

在项目目录 `src/main/` 下新建文件夹`resources`，接着在`resources`下新建 MyBatis 配置文件 `mybatis.cfg.xml` ，用来配置 Mybatis 的运行环境、数据源、事务等。

mybatis.cfg.xml 的配置如下，具体解释注释已经给出：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
       <!-- 配置 mybatis 运行环境 -->
    <environments default="development">
        <environment id="development">
           <!-- type="JDBC" 代表直接使用 JDBC 的提交和回滚设置 -->
            <transactionManager type="JDBC" />

            <!-- POOLED 表示支持 JDBC 数据源连接池 -->
            <!-- 数据库连接池，由 Mybatis 管理，数据库名是 mybatis，MySQL 用户名 root，密码为空 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis" />
                <property name="username" value="root" />
                <property name="password" value="" />
            </dataSource>
        </environment>
    </environments>
</configuration>
```

### 2.6实体类USer

在包 `com.shiyanlou.mybatis.model` 下新建类 User.java ， 一个用户具有：id、username、password、sex、address 五个属性。作为 mybatis 进行 sql 映射使用，与数据库表对应。

User 类的代码如下：

```java
package com.shiyanlou.mybatis.model;

public class User {

    private Integer id;
    private String username;
    private String password;
    private String sex;
    private String address;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

}
```

### 2.7创建方法接口和定义映射文件

新建包 `com.shiyanlou.mybatis.mapper` ，并在包下新建方法接口 UserMapper.java ，提供简单的增删改查数据操作。

UserMapper 接口的代码如下：

```java
package com.shiyanlou.mybatis.mapper;

import java.util.List;

import com.shiyanlou.mybatis.model.User;

public interface UserMapper {

    /*
     * 新增用戶
     * @param user
     * @return
     * @throws Exception
     */
    public int insertUser(User user) throws Exception;

    /*
     * 修改用戶
     * @param user
     * @param id
     * @return
     * @throws Exception
     */
    public int updateUser(User user) throws Exception;

    /*
     * 刪除用戶
     * @param id
     * @return
     * @throws Exception
     */
    public int deleteUser(Integer id) throws Exception;

    /*
     * 根据 id 查询用户信息
     * @param id
     * @return
     * @throws Exception
     */
    public User selectUserById(Integer id) throws Exception;

    /*
     * 查询所有的用户信息
     * @return
     * @throws Exception
     */
    public List<User> selectAllUser() throws Exception;
}
```

在包 `com.shiyanlou.mybatis.mapper` 下新建映射文件 `UserMapper.xml` ，用来定义各种 SQL 语句和这些语句的参数，以及要返回的类型等。

UserMapper.xml 的配置如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org/DTD Mapper 3.0" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shiyanlou.mybatis.mapper.UserMapper">
    <!-- 自定义返回结果集 -->
    <resultMap id="userMap" type="User">
        <id property="id" column="id" javaType="int"></id>
        <result property="username" column="username" javaType="String"></result>
        <result property="password" column="password" javaType="String"></result>
        <result property="sex" column="sex" javaType="String"></result>
        <result property="address" column="address" javaType="String"></result>
    </resultMap>

    <!-- 定义 SQL 语句，其中 id 需要和接口中的方法名一致 -->
    <!-- useGeneratedKeys：实现自动生成主键 -->
    <!-- keyProperty： 唯一标记一个属性 -->
    <!-- parameterType 指明查询时使用的参数类型，resultType 指明查询返回的结果集类型 -->
    <insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
        insert into user (username,password,sex,address) values
        (#{username},#{password},#{sex},#{address})
    </insert>

    <update id="updateUser"  parameterType="User">
        update user set
        address=#{address} where
        id=#{id}
    </update>

    <delete id="deleteUser" parameterType="int">
        delete from user where
        id=#{id}
    </delete>

    <!-- 如未为 Java Bean 起类别名，resultType="com.shiyanlou.mybatis.model.User" -->

    <!-- 使用 resultType 时，一定要保证，你属性名与字段名相同；如果不相同，就使用 resultMap -->
    <select id="selectUserById" parameterType="int" resultType="User">
        select * from user where id=#{id}
    </select>

    <select id="selectAllUser" resultMap="userMap">
        select * from user
    </select>

</mapper>
```

UserMapper.xml 映射文件写好后，需要在 mybatis.cfg.xml 中加载它，在 mybatis.cfg.xml 中添加代码：

```xml
<mappers>
    <!-- 通过 mapper 接口包加载整个包的映射文件 -->
    <package name="com.shiyanlou.mybatis.mapper" />
</mappers>
```

同时为 Java Bean 起别名，添加如下代码：

```xml
<!-- 为 JavaBean 起类别名 -->
<typeAliases>
    <!-- 指定一个包名起别名，将包内的 Java 类的类名作为类的类别名 -->
    <package name="com.shiyanlou.mybatis.model" />
</typeAliases>
```

注意顺序：typeAliases->environments->mappers
![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542080079878.png/wm)

### 2.8日志记录log4j.properties

使用日志文件是为了查看控制台输出的 SQL 语句。

在项目目录 `src/main/resources` 下新建 MyBatis 日志记录文件 log4j.properties ，在里面添加如下内容：

```
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

### 2.9测试类UserTest

在包 `com.shiyanlou.mybatis.test` 下新建测试类 `UserTest.java` ， 用来测试数据的增删改查操作。

UserTest 类的代码如下：

```java
package com.shiyanlou.mybatis.test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import com.shiyanlou.mybatis.mapper.UserMapper;
import com.shiyanlou.mybatis.model.User;

public class UserTest {
    private static SqlSessionFactory sqlSessionFactory;

    public static void main(String[] args) {
        // Mybatis 配置文件
        String resource = "mybatis.cfg.xml";

        // 得到配置文件流
        InputStream inputStream = null;
        try {
            inputStream = Resources.getResourceAsStream(resource);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 创建会话工厂，传入 MyBatis 的配置文件信息
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        insertUser();
        // updateUser();
        // deleteUser();
        // selectUserById();
        // selectAllUser();

    }

    // 新增用戶
    private static void insertUser() {
        // 通过工厂得到 SqlSession
        SqlSession session = sqlSessionFactory.openSession();

        UserMapper mapper = session.getMapper(UserMapper.class);
        User user = new User();
        user.setUsername("Tom");
        user.setPassword("123456");
        user.setSex("male");
        user.setAddress("chengdu");
        try {
            mapper.insertUser(user);

            session.commit();
        } catch (Exception e) {
            e.printStackTrace();
            session.rollback();
        }

        // 释放资源
        session.close();
    }

    // 更新用戶
    private static void updateUser() {

        SqlSession session = sqlSessionFactory.openSession();

        UserMapper mapper = session.getMapper(UserMapper.class);
        User user = null;
        try {
            user = mapper.selectUserById(1);
        } catch (Exception e1) {
            e1.printStackTrace();
        }
        user.setAddress("chongqing");
        try {
            mapper.updateUser(user);
            session.commit();
        } catch (Exception e) {
            e.printStackTrace();
            session.rollback();
        }

        session.close();
    }

    // 删除用戶
    private static void deleteUser() {

        SqlSession session = sqlSessionFactory.openSession();

        UserMapper mapper = session.getMapper(UserMapper.class);
        try {
            mapper.deleteUser(3);
            session.commit();
        } catch (Exception e) {
            e.printStackTrace();
            session.rollback();
        }

        session.close();
    }

    // 根据 id 查询用户信息
    private static void selectUserById() {

        SqlSession session = sqlSessionFactory.openSession();

        UserMapper mapper = session.getMapper(UserMapper.class);
        try {
            User user = mapper.selectUserById(1);
            session.commit();
            System.out.println(user.getId() + " " + user.getUsername() + " "
                    + user.getPassword() + " " + user.getSex() + " "
                    + user.getAddress());
        } catch (Exception e) {
            e.printStackTrace();
            session.rollback();
        }

        session.close();
    }

    // 查询所有的用户信息
    private static void selectAllUser() {

        SqlSession session = sqlSessionFactory.openSession();

        UserMapper mapper = session.getMapper(UserMapper.class);
        try {
            List<User> userList = mapper.selectAllUser();
            session.commit();
            for (User user : userList) {
                System.out.println(user.getId() + " " + user.getUsername() + " "
                        + user.getPassword() + " " + user.getSex() + " "
                        + user.getAddress());
            }
        } catch (Exception e) {
            e.printStackTrace();
            session.rollback();
        }

        session.close();
    }
}
```

### 2.10运行测试

分别调用 UserTest 类中的增删改查方法。使用下面的命令就可以运行`UserTest.java`

```
mvn compile
mvn exec:java -Dexec.mainClass="com.shiyanlou.mybatis.test.UserTest" -Dexec.cleanupDaemonThreads=false
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542086657432.png/wm)

结果如下：

#### 3.9.1 insertUser()

插入一条用户数据：

（1）控制台输出

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542080117447.png/wm)

（2）查看数据库

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542080273763.png/wm)

#### 3.9.2 updateUser()

将 id 为 1 的用户的 address 修改为 chongqing：

（1）控制台输出

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542089025145.png/wm)

（2）查看数据库

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542089061492.png/wm)

#### 3.9.3 deleteUser()

在进行测试之前，再往数据库表中插入两行用户记录。

```
insert into user (id,username,password,sex,address) values (2,"Jack","13579","male","beijing"),(3,"Rose","24680","female","shanghai");
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542089467492.png/wm)

将 id 为 3 的用户删除：

（1）控制台输出

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542089598891.png/wm)

（2）查看数据库

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542089619263.png/wm)

#### 3.9.4 selectUserById()

查询 id 为 1 的用户信息：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542090126123.png/wm)

#### 3.9.5 selectAllUser()

查询所有用户信息：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542090324778.png/wm)

