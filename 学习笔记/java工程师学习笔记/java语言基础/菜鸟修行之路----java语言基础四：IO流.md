菜鸟修行之路：java语言基础四：IO流

​      输入输出（I/O）是计算机的最基本操作。例如：键盘输入数据，文件的读取与写入等等。

​     对于文件操作，Java通过它的输入输出（I/O）类库（java.io）来实现。

**流**：一个可被顺序访问的数据序列，是对计算机输入数据和输出数据的抽象。

根据运行方向，流可以分为2类：

- 输入流   将外部数据引入到计算机CPU中，例如：从磁盘中读取数据等
- 输出流   将数据引导到外部设备，例如：向磁盘中保存文件

所以综上所述：流就是数据从一种设备流向另外一种设备的过程，知识这个过程中的每一个数据的输入输出顺序是固定的。

## 1.IO类库

流序列中的数据主要有2类：

- 字节流   原始的二进制数据
- 字符流   一定编码处理后符合某种格式规定的特定数据。

在java.io包中有4个基本类用于输入输出流的操作：

| 类          | 功能             |
| ----------- | ---------------- |
| InputStream | 处理字节流的输入 |
| OuputStream | 处理字节流的输出 |
| Reader      | 处理字符流的读取 |
| Writer      | 处理字符流的写入 |

**注**：

1. 这四大基流都是抽象类，其他流都是继承于这四大基流的。 
2.  字节流：数据流中最小的数据单元是字节 
3. 字符流：数据流中最小的数据单元是字符， Java中的字符是Unicode编码，一个字符占用两个字节（**无论中文还是英文都是两个字节**）。

### 1.1 iO流整体架构图

![1583030680866](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\1583030680866.png)

1.2 常见字节流

### 1.2 字节流与字符流的相互转化：

​        在实际应用中很多数据是文本的，所以提出了字符流的概念，按照16位Unicode码来处理字符数据，并且完成字节型数据与字符型数据的转化。

​       在java中使用以下2个类完成字节与字符的转化：

- InputStreamReader：将读取的字节型数据解码为字符型数据。

  ​          实质：字节数组byte[]转化字符串String。

  ```java
  public String (byte bytes[],String charsetName);
  ```

- OuputStreamWriter：将写入的字符型数据编码为字节型数据。

  ​        实质：字符串String转化为字节数组byte[]。

  ```java
  byte[] String.getBytes(String charsetName);
  ```

### 1.3 操作IO流

基本步骤：

①、创建源或目标对象

　　　　输入：把文件中的数据流向到程序中，此时文件是 源，程序是目标

　　　　输出：把程序中的数据流向到文件中，此时文件是目标，程序是源

②、创建 IO 流对象

　　　　输入：创建输入流对象

　　　　输出：创建输出流对象

③、具体的 IO 操作

④、关闭资源

　　输入：输入流的 close() 方法

　　输出：输出流的 close() 方法

具体实例：

​      参见下文文件流操作实例：文件拷贝

## 2.文件处理

​       在Java语言的java.io包中，由File类提供了描述文件和目录的操作与管理方法。

​       File类不是InputStream、OutputStream或Reader、Writer的子类，因为它不负责数据的输入输出，而专门用来管理磁盘文件与目录。（与之类似还有socket）

**File 类：文件和目录路径名的抽象表示。**

**注意：File 类只能操作文件的属性，所以通过File类不能操作文件的内容。对于文件内容的读写采用专门的文件流操作类实现。**

### 2.1File类

​     通过File类我们可以管理磁盘文件与目录，常用于获取文件以及目录属性，文件以及目录的创建和删除。

​    File常用方法：

**文件以及目录的创建**：

| 方法                    | 功能                                           |
| ----------------------- | ---------------------------------------------- |
| boolean createNewFile() | 文件创建，不存在返回true 存在返回false         |
| boolean mkdir()         | 创建目录，如果上一级目录不存在，则会创建失败   |
| boolean mkdirs()        | 创建多级目录，如果上一级目录不存在也会自动创建 |

**文件以及目录的删除**：

| 方法                   | 功能                                                   |
| ---------------------- | ------------------------------------------------------ |
| boolean delete()       | 删除文件或目录，如果表示目录，则目录下必须为空才能删除 |
| boolean deleteOnExit() | 文件使用完成后删除                                     |

**文件以及目录的属性判断**：

| 方法                  | 功能                                   |
| --------------------- | -------------------------------------- |
| boolean canExecute()  | 判断文件是否可执行                     |
| boolean canRead()     | 判断文件是否可读                       |
| boolean canWrite()    | 判断文件是否可写                       |
| boolean exists()      | 判断文件或目录是否存在                 |
| boolean isDirectory() | 判断此路径是否为一个目录               |
| boolean isFile()      | 判断是否为一个文件                     |
| boolean isHidden()    | 判断是否为隐藏文件                     |
| boolean isAbsolute()  | 判断是否是绝对路径或者判断文件是否存在 |

**文件以及目录属性获取**：

| 方法                                 | 功能                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| String getName()                     | 获取此路径表示的文件或目录名称                               |
| String getPath()                     | 将此路径名转换为路径名字符串                                 |
| String getAbsolutePath()             | 返回此绝对路径名的绝对形式                                   |
| String getParent()                   | 获取父目录路径，如果没有父目录返回null                       |
| long lastModified()                  | 获取最后一次修改的时间                                       |
| long length()                        | 返回由此路径名表示的文件的长度。                             |
| boolean renameTo(File f)             | 重命名由此路径名表示的文件                                   |
| File[] liseRoots()                   | 获取机器盘符                                                 |
| String[] list()                      | 返回一个字符串数组，命名由此路径名表示的目录中的文件和目录。 |
| String[] list(FilenameFilter filter) | 返回一个字符串数组，命名由此抽象路径名表示的目录中满足指定过滤器的文件和目录。 |

**文件简单操作实例：获取给定目录下的所有文件夹和文件夹里面的内容**

```java
public class  PrintFile{
    public static void getFileList(File file){
         //第一级子目录
         File[] files = file.listFiles();
        //遍历文件数组
         for(File f:files){
              //打印目录和文件
              System.out.println(f);
              if(f.isDirectory()){
                  getFileList(f);
              }
         }
    }
    public static void main(String[] args) throws Exception {
         File f = new File("D:"+File.separator+"WebStormFile");
         getFileList(f);
   }
}
```

### 2.2 文件流

文件流和前面的IO流类似，也提供了4个基本类用于文件的读写：

- FileInputStream  字节文件输入流
- FileOutputStream 字节文件输出流
- FileReader   字符文件输入流
- FileWriter     字符文件输出流　

#### 2.2.1 FileInputStream/FileOutputStream 

FileInputStream  继承自InputStream类，用于处理二进制文件的输入操作。

FileOutputStream继承自OutputStream类，用于处理二进制文件的输出操作

**FileInputStream构造方法**：

- FileInputStream(File file) ：指定的文件对象。
- FileInputStream(String name) ：指定的文件名，包括路径
- FileInputStream（FileDescripter fd） :指定的文件描述符。

注：文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向[内核](https://baike.baidu.com/item/内核)为每一个进程所维护的该进程打开文件的记录表。

**FileInputStream常用方法**：

| 方法                | 功能                                                         |
| ------------------- | ------------------------------------------------------------ |
| int read()          | 一次读取一个字节<br/>返回值：下一个数据字节；如果已到达文件末尾，则返回 -1。 |
| int read(byte[] b)  | 一次读取一个字节数组 (读取实际的字节数)     指定字节数组的长度是:1024或者1024的倍数<br/>返回：读入缓冲区的字节总数，如果因为已经到达文件末尾而没有更多的数据，则返回 -1。<br/>b - 存储读取数据的缓冲区 |
| public void close() | 关闭此文件输入流并释放与此流有关的所有系统资源。             |

**OutputStream构造方法**：参数含义与FileInputStream  一致。

**FileOutputStream常用方法**

| 方法                          | 功能                                                 |
| ----------------------------- | ---------------------------------------------------- |
| write(int b)                  | 将指定字节写入此文件输入流                           |
| write(byte[] b)               | 将字节数组写入此文件输入流                           |
| write(byte[],int off,int len) | 将指定字节数组从off开始后的len个字节写入此文件输入流 |

字符流与字节流文件构造方法，操作方法类似。具体参见API文档。

#### 2.2.2 文件操作实例--文件拷贝

**使用字节流实现文件拷贝**:

注：文件操作参见下文。

```java
public class Copy{
    public static void main(String[] args){
        FileInputStream in =null;
        FileOutputStream out=null;
        //创建文件目录
        File file=new File("文件复制目标目录");
        //创建新文件
        try {
            file.createNewFile();
            //构造输入流，读取数据
            in =new FileInputStream("源文件目录");
            //构造输出流，将输入流读取的文件输出到新文件中
            out=new FileOutputStream(file);
            //采用遍历，逐个读取，存入字节，实现文件拷贝
            int fi;
            while((fi=in.read())!=-1){
                out.write(fi);
            }
            //关闭资源
            in.close();
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

注：对于文件读写操作可能存在异常，所以文件操作语句放在try..catch语句中。

**使用字符流实现文件拷贝**:

```java
public class Copy{
    public static void main(String[] args){
        FileReader in =null;
        FileWriter out=null;
        //创建文件目录
        File file=new File("文件复制目标目录");
        try {
            //创建新文件
            file.createNewFile();
            //构造输入流，读取数据
            in =new FileReader("源文件目录");
            //构造输出流，将输入流读取的文件输出到新文件中
            out=new FileWriter(file);
            //采用遍历，逐个读取，实现文件拷贝
            int fi;
            while((fi=in.read())!=-1){
                out.write(fi);
            }
            //关闭资源
            in.close();
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```







**简单小结：**

​      2020年世界没有开一个好头，但是2020还有一整年，希望我们有一个充满希望的过程以及漂亮的结尾！！！

​     时光如水，这个是我的第四篇博客，希望以此记录我的修行岁月。修行总是艰辛痛苦的，但是我相信一定会有一个美好的结果。

​     2020的目标：生命不息，修行不止，每日一记不停！！！！！！！

**修行之路艰辛，与君共勉。**

​                                                                                                                                                             ---2020年春：成都

　　　　1.String getName() 获取此路径表示的文件或目录名称
　　　　2.String getPath() 将此路径名转换为路径名字符串
　　　　3.String getAbsolutePath() 返回此抽象路径名的绝对形式
　　　　4.String getParent()//如果没有父目录返回null
　　　　5.long lastModified()//获取最后一次修改的时间
　　　　6.long length() 返回由此抽象路径名表示的文件的长度。
　　　　7.boolean renameTo(File f) 重命名由此抽象路径名表示的文件。
　　　　8.File[] liseRoots()//获取机器盘符
　　　　9.String[] list()  返回一个字符串数组，命名由此抽象路径名表示的目录中的文件和目录。
　　　　10.String[] list(FilenameFilter filter) 返回一个字符串数组，命名由此抽象路径名表示的目录中满足指定过滤器的文件和目录。