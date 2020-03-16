# 菜鸟修行之路----java语言基础三：异常

​        异常（Exception）指程序运行过程中出现的非正常现象。

## 1.Java异常分类

在java中，异常分为2类：异常（Exception）和错误（Error）。

**Throwable** 是 Java 语言中所有错误或异常的超类。具体结构如下图所示：

![1582937812111](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1582937812111.png)

### 1.1  Error

​        Error 类是指 java 运行时系统的内部错误和资源耗尽错误。由系统保留，不会抛出该类对象。

​        Error类的对象不可捕获，不可恢复，出错时系统通知用户并且终止程序。

### 1.2  Exception

​     Exception类供应用程序使用，所有的Java异常类都是该类的子类。

​     Exception 又 有 两 个 分 支 ：

-  **RuntimeException**   运行时异常
-  **CheckedException**   编译时异常

**RuntimeException** 

 如 ： NullPointerException 、 ClassCastException ；

 RuntimeException 是 那些可能在 Java 虚拟机正常运行期间抛出的异常的超类。 

**CheckedException**：

一般是外部错误，这种异常都发生在编译阶段，例如 I/O 错误导致的 IOException、SQLException。

Java 编译器会强 制程序去捕获此类异常，即会出现要求你把这段可能出现异常的程序进行 try catch，该类异常一 般包括几个方面： 

- 试图在文件尾部读取数据 
- 试图打开一个错误格式的 URL  
- 试图根据给定的字符串查找 class 对象，而这个字符串表示的类并不存在

**常见的系统定义的异常**：

| 系统定义的异常            | 说明                                                         |
| ------------------------- | ------------------------------------------------------------ |
| NullPointerException      | 空指针异常，调用了未经初始化的对象或者是不存在的对象         |
| ClassNotFoundException    | 指定的类找不到，类的名称和路径加载错误；<br/>通常都是程序试图通过字符串来加载某个类时可能引发异常。 |
| NumberFormatException     | 字符串转换为数字异常，字符型数据中包含非数字型字符。         |
| IndexOutOfBoundsException | 数组角标越界异常，常见于操作数组对象时发生。                 |
| IllegalArgumentException  | 方法传递参数错误。                                           |
| ClassCastException        | 数据类型转换异常。                                           |
| NoClassDefFoundException  | 未找到类定义错误。                                           |
| SQLException              | SQL 异常，常见于操作数据库时的 SQL 语句错误。                |
| InstantiationException    | 实例化异常。                                                 |
| NoSuchMethodException     | 方法不存在异常                                               |
| ArithmeticException       | 算术错误，例如除数为0                                        |

### 1.3 用户自定义异常

系统定义的异常通常用于处理系统可以预见的常见的运行错误，对于一些特定的异常问题，需要我们根据程序的特殊逻辑，自定义已异常情况类和异常对象。

自定义异常步骤：

1. 声明一个新的异常类，并且继承Exception类或者其他已经存在的系统异常类以及其他用户异常类（根据异常情况选择）。
2.  为新的异常类定义属性和方法，或者重载父类的属性和方法。

**简单实例**：

```java
/**
 * 用户自定义异常类
 */
public class UserException extends Exception {
    //异常信息
    private String message;

    //构造函数
    public UserException(String message){
        super(message);
        this.message = message;
    }

    /**
    *获取异常信息
    构造函数调用了super(message),不用重写此方法
    */
    //public String getMessage(){
    //    return message
    //}
}

```

## 2.异常的抛出

异常的抛出：应用程序在运行时出现了可识别错误，就会产生一个与该错误的对应的异常类对象。

被抛出的异常对象包含了异常的类型和错误出现的程序的状态信息。

异常抛出方法也分为2类：

1. 系统自动抛出的异常
2. 声明抛出异常

**系统自动抛出的异常**：

所有系统定义的异常都可以由系统自动抛出。

简单实例：

```java
class Exception_01{
    public static void main(String[] args){
        int a=5,b=0;
        //这里系统将会自动抛出一个算术错误ArithmeticException的对象。
        System.out.print(a/d);
    }
}
```

**声明抛出异常**

用户自定义的异常不能靠系统自动抛出，必须通过throw语句来定义异常错误，并且抛出这个异常对象。

具体使用格式：

```java
修饰符 返回类型 方法名（） throws 异常类名{
    ...
    Throws 异常类名;
    ...     
}
```

具体实例：

```java
public int read() throws IOException{
    ...;
    IOException e=new IOException();
    ...;
}
```

## 3 .异常处理

对于抛出的异常处理，一般包括以下几方面：

- 捕捉异常
- 程序流程的跳转
- 异常处理语句块的定义

对于异常的处理，一般采用try....catch...finally结构来对异常进行捕获和处理。具体格式如下：

```java
try{
    可能出现异常的程序代码块
}catch(异常类1 变量1){ 异常类1 对应的异常处理代码}
 catch(异常类2 变量2){ 异常类2 对应的异常处理代码}
...
   finally{
       无论异常是否发生都要执行的代码
   }

```

**说明**：

- 每一个catch子句用来识别并且捕获产生的异常，但是程序每一次运行只能由一个catch语句工作，就是说每一次运行只能识别一个异常。
- 通过finally语句提供统一出口。所以一般在finally中做一些善后操作，例如：关闭数据库连接，关闭文件，释放资源等。

## 4.异常应用实例

通过一个登录操作的密码验证的异常实例来复习一个完整的用户自定义异常的创建，抛出以及处理。

**创建自定义异常类**：

```java
/**
 * 用户自定义异常类
 */
public class UserException extends Exception {
    //异常信息
    private String message;

    //构造函数
    public UserException(String message){
        super(message);
        this.message = message;
    }

    /**
    *获取异常信息
    构造函数调用了super(message),不用重写此方法
    */
    //public String getMessage(){
    //    return message
    //}
}

```

**抛出异常**：

```java
/**
*在需要抛出异常的地方使用自定义的异常类。
*/
public class User{
    private String name;
    private String password;
    
    public User (String name,String password){
        this,name=name;
        this.password=password;
    }
    
   publice void throwException(String password) throws UserException{
       if(!this.password.equals(password)){
           throw new UserException("密码输入不正确，请重新输入")
       }
   }
}
```

**处理异常**：

```java
public class UserTest{
    public static void main(String[] args){
        User user=new User("admin","123");
        try{
            user.throwException("1234");
        }catch(UserException ue){
            System.out.println("UserException"+ue.getMessage());
        }
    }
}
```



**后记：修行之路艰辛，与君共勉。**

​                                                                                                                                                             ---2020年春：成都