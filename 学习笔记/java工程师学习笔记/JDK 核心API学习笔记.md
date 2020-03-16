# JDK 核心API学习笔记

## 一.API概述

Java 的核心 API 是非常庞大的，这给开发者来说带来了很大的方便。所谓的 API 就是一些已经写好、可直接调用的类库。Java 里有非常庞大的 API，其中有一些类库是我们必须得掌握的，只有熟练掌握了 Java 一些核心的 API，我们才能更好的使用 Java。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid18510labid1118timestamp1500451929664.png/wm)

在程序中，java.lang 包并不需要像其他包一样需要`import`关键字进行引入。系统会自动加载，所以我们可以直接取用其中的所有类。下面我们就来详细地学习一下 java.lang 包吧

## 二. java.lang包装类

我们都知道 java 是一门面向对象的语言，类将方法和属性封装起来，这样就可以创建和处理相同方法和属性的对象了。但是 java 中的基本数据类型却不是面向对象的，不能定义基本类型的对象。那如果我们想像处理对象的方式处理基本类型的数据，调用一些方法怎么办呢？

其实 java 为每个基本类型都提供了包装类：

| 原始数据类型      | 包装类    |
| ----------------- | --------- |
| byte（字节）      | Byte      |
| char（字符）      | Character |
| int（整型）       | Integer   |
| long （长整型）   | Long      |
| float（浮点型）   | Float     |
| double （双精度） | Double    |
| boolean （布尔）  | Boolean   |
| short（短整型）   | Short     |

在这八个类名中，除了 Integer 和 Character 类以后，其它六个类的类名和基本数据类型一致，只是类名的第一个字母大写。

#### 2.2.1 Integer 类

java.lang 包中的 Integer 类、Long 类和 Short 类都是 Number 的子类，他们的区别在于不同子类里面封装着不同的数据类型，比如 Integer 类包装了一个基本类型 int。其包含的方法基本相同。

我们以 Integer 类为例。 Integer 构造方法有两种：

1. Integer(int value) ，以 int 型变量作为参数创建 Integer 对象。例如`Integer a = new Integer(10);`
2. Integer(String s) ，以 String 型变量作为参数创建 Integer 对象，例如`Integer a = new Integer("10")`

下面列举一下 Integer 的常用方法

| 方法                              | 返回值  | 功能描述                                                     |
| --------------------------------- | ------- | ------------------------------------------------------------ |
| byteValue()                       | byte    | 以 byte 类型返回该 Integer 的值                              |
| compareTo(Integer anotherInteger) | int     | 在数字上比较 Integer 对象。如果这两个值相等，则返回 0；如果调用对象的数值小于 anotherInteger 的数值，则返回负值；如果调用对象的数值大于 anotherInteger 的数值，则返回正值 |
| equals(Object IntegerObj)         | boolean | 比较此对象与指定对象是否相等                                 |
| intValue()                        | int     | 以 int 型返回此 Integer 对象                                 |
| shortValue()                      | short   | 以 short 型返回此 Integer 对象                               |
| longValue()                       | long    | 以 long 型返回此 Integer 对象                                |
| floatValue()                      | float   | 以 float 型返回此 Integer 对象                               |
| doubleValue()                     | double  | 以 double 型返回此 Integer 对象                              |
| toString()                        | String  | 返回一个表示该 Integer 值的 String 对象                      |
| valueOf(String str)               | Integer | 返回保存指定的 String 值的 Integer 对象                      |
| parseInt(String str)              | int     | 将字符串参数作为有符号的十进制整数进行解析                   |

我们来写一些代码，验证一下上面的方法吧，在 project 下新建 Java 文件`IntegerTest.java`

```java
public class IntegerTest {
    public static void main(String[] args){
        //初始化一个 Integer 类实例
        Integer a = new Integer("10");
        //初始化一个 Integer 类实例
        Integer b = new Integer(11);
        //判断两个数的大小
        System.out.println(a.compareTo(b));
        // 判断两个实例是否相等
        System.out.println(a.equals(b));
        //将 a 转换成 float 型数
        float c = a.floatValue();
        System.out.println(c);

        String d = "10101110";
        //将字符串转换为数值
        //parseInt(String str) 和 parseInt(String str,int radix) 都是类方法，由类来调用。后一个方法则实现将字符串按照参数 radix 指定的进制转换为 int，
        int e = Integer.parseInt(d, 2);
        System.out.println(e);
    }
}
```

打开 terminal，输入命令

```shell
javac IntegerTest.java
java IntegerTest
```

输出结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542939596994.png/wm)

大家自己试一试吧。

#### 2.2.2 Character 类

Character 类在对象中包装一个基本类型 char 的值。Character 类型的对象包含类型为 char 的单个字段。

Character 包装类的常用方法：

| 方法                              | 返回值  | 说明                                                |
| --------------------------------- | ------- | --------------------------------------------------- |
| isDigit(char ch)                  | boolean | 确定字符是否为数字                                  |
| isLetter(char ch)                 | boolean | 确定字符是否为字母                                  |
| isLowerCase(char ch)              | boolean | 确定字符是否为小写字母                              |
| isUpperCase(char ch)              | boolean | 确定字符是否为大写字母                              |
| isWhitespace(char ch)             | boolean | 确定字符是否为空白字符                              |
| isUnicodeIdentifierStart(char ch) | boolean | 确定是否允许将指定字符作为 Unicode 标识符中的首字符 |

大家可以参考一下代码，验证一下上面的方法（同学们一定要亲自打哦，学代码的最好方式是实操） 在 project 文件下创建 CharacterTest.java 文件

```java
public class CharacterTest {
    public static void main(String[] args){
        int count;
        //定义了一个字符数组
        char[] values = {'*', '_', '%', '8', 'L', 'l'};
        //遍历字符数组
        for (count = 0; count < values.length; count++){
            if(Character.isDigit(values[count])){
                System.out.println(values[count]+"是一个数字");
            }
            if(Character.isLetter(values[count])){
                System.out.println(values[count]+ "是一个字母");
            }
            if(Character.isUpperCase(values[count])){
                System.out.println(values[count]+"是大写形式");
            }
            if(Character.isLowerCase(values[count])){
                System.out.println(values[count]+"是小写形式");
            }
            if(Character.isUnicodeIdentifierStart(values[count])){
                System.out.println(values[count]+"是 Unicode 标志符的第一个有效字符");
            }
        }
        //判断字符 c 是否是空白字符
        char c = ' ';
        System.out.println("字符 c 是空白字符吗？"+Character.isWhitespace(c));
    }
}
```

输出结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542939891404.png/wm)

#### 2.2.3 Boolean 类

Boolean 类将基本类型为 boolean 的值包装在一个对象中。一个 Boolean 类型的对象只包含一个类型为 boolean 的字段。

Boolean 类的构造方法也有两个：

1. Boolean(boolean value)，创建一个表示 value 参数的 Boolean 对象，如`Boolean b = new Boolean(true)`
2. Boolean(String s)，如果 String 参数不为 null 且在忽略大小写时等于 "true", 创建一个表示 true 值的 Boolean 对象，如`Boolean b = new Boolean("ok")`，为 false。

Boolean 包装类的常用方法：

| 方法                   | 返回值  | 说明                                                         |
| ---------------------- | ------- | ------------------------------------------------------------ |
| booleanValue()         | boolean | 将 Boolean 对象的值以对应的 boolean 值返回                   |
| equals(Object obj)     | boolean | 判断调用该方法的对象与 obj 是否相等。当且仅当参数不是 null，而且与调用该方法的对象一样都表示同一个 boolean 值的 Boolean 对象时，才返回 true |
| parseBoolean(String s) | boolean | 将字符串参数解析为 boolean 值                                |
| toString()             | String  | 返回表示该 boolean 值的 String 对象                          |
| valueOf(String s)      | Boolean | 返回一个用指定得字符串表示值的 boolean 值                    |

我们来写一些代码，验证一下上面的方法吧 在 project 下创建 BooleanTest.java

```java
public class BooleanTest {
    public static void main(String[] args) {
        // Boolean(boolean value) 构造方法
        Boolean a = new Boolean(true);
        System.out.println("a 为"+a);
        // Boolean(String s) 构造方法
        Boolean b = new Boolean("true");
        Boolean c = new Boolean("OK");
        System.out.println("b 为"+b);
        System.out.println("c 为"+c);
        System.out.println("a 的 booleanValue() 为"+a.booleanValue());
        System.out.println("a 和 b 相等吗？"+a.equals(b));
    }
}
```

输出结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1542939947328.png/wm)

其他的都差不多，大家可以自行去验证和参考 Java 类库的手册吧。