###一、Java流式原理

- 先上一个图：

![1552823056887](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552823056887.png)

- 流：类比生活中的水流

- **在计算机中所有的数据都是以文件的形式存储，同时又是以文件的形式传输，**

  **在java中也是以流的形式来传输数据**

###二、File类

- 在java中封装了一个File类来实现数据的传输，不管是文件、文件夹或是网络上流动的数据都是File类或是File的子集

####1、创建一个文件对象

使用相对路径和绝对路径创建文件对象

> ①相对路径创建文件
>
> ```java
> package testfile;
> 
> import java.io.File;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/13 20:09
> */
> public class TestFile {
>  public static void main(String[] args) {
>      File file=new File("test");
>      File file1 = new File("demo/d");
>      if (!file.exists()) {
>          System.out.println(file.mkdir());//创建由此抽象路径名命名的目录
>      }else {
>          System.out.println("file.delete() = " + file.delete());
>      }
> 
>      if (!file1.exists()) {
>          //创建由此抽象路径名命名的目录，包括任何必需但不存在的父目录
>          System.out.println("file1.mkdirs() = " + file1.mkdirs());
>      }else{
>          System.out.println("file1.delete() = " + file1.delete());
>      }
>  }
>     
> }
> ```
>
> 

②绝对路径创建文件

> ```java
> package testfile;
> 
> import java.io.File;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/13 20:09
> */
> public class TestFile {
> 
>  public static void main(String[] args) {
>      File file = new File("G:/demo/d");
>      if (!file.exists()) {
>          System.out.println("file.mkdirs() = " + file.mkdirs());//换成mkdir()类似
>      }
>      System.out.println("file.getAbsolutePath() = " + file.getAbsolutePath());
>      System.out.println("file.getParentFile() = " + file.getParentFile());
>      System.out.println("file.isFile() = " + file.isFile());
>      System.out.println("file.isDirectory() = " + file.isDirectory());
>      System.out.println("file.getName() = " + file.getName());
> 
>      File file1 = new File("G:/test");
>      if (!file1.exists()) {
>          file1.mkdirs();
>      }
>      //以file1为父目录创建file2
>      File file2 = new File(file1, "test");
>      if (!file2.exists()) {
>          file2.mkdirs();
>      }
>      System.out.println("file1.getAbsolutePath() = " + file2.getAbsolutePath());
>  }
> }
> /**
> 运行结果：
> file.getAbsolutePath() = G:\demo\d
> file.getParentFile() = G:\demo
> file.isFile() = false
> file.isDirectory() = true
> file.getName() = d
> file1.getAbsolutePath() = G:\test\test
> */
> ```

####2、文件常用方法1

```java
@Test
    public void test() {

        File file = new File("G:/test/ideaIU-2018.3.3.exe");
        System.out.println("file.exists() = " + file.exists());
        System.out.println("file.isDirectory() = " + file.isDirectory());
        System.out.println("file.isFile() = " + file.isFile());
        System.out.println("file.isHidden() = " + file.isHidden());
        System.out.println("file.length() = " + file.length());

        File file1 = new File("G:/test");
        System.out.println("file1.list() = " + file1.list());
        System.out.println("file1.listFiles() = " + file1.listFiles());

        File file2 = new File("G:/test/test.txt");
        File file3 = new File("G:/test/demo.txt");
        System.out.println("file2.exists() = " + file2.exists());
        long time=file2.lastModified();
        Date date = new Date(time);
        System.out.println("file2.lastModified() = " + date);
        System.out.println("file2.setLastModified(0) = " + file2.setLastModified(0));
        System.out.println("file2.renameTo(file3) = " + file2.renameTo(file3));
    }
```

运行结果：

![运行结果](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552537656583.png)



####3、文件常用方法2

####4、遍历文件夹

####5、遍历子文件夹

### 三、输入输出流分类

**所谓输入输出就是数据的读写（进出），可以类比河道内水流的流动（流出与流入）**

|            |    字节流    | 字符流 |
| :--------: | :----------: | :----: |
| **输入流** | InputStream  | Reader |
| **输出流** | OutputStream | Writer |

- **字节流：**以单个字节的方式进行读写
- **字符流：**以单个字符为单位进行读取，
- java中一个英文字符占一个字节，一个中文字符占两个字节
- **在开发中输入输出流是站在程序的角度而言**

### 四、节点流与数据流

> - **节点流：**指**直接**对指定的数据源进行操作
> - **处理流：**是指连接在**已有的流（节点流或数据流）**之上，为程序提供更为强大的读写功能的流。这里用**连接来**形容流是因为数据源与程序之间必须先建立一个用于数据传输的通道才能进行数据的传输（**如：HTTP连接**）
> - ![1552826167246](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552826167246.png)

​								

>​                                                                            **节点流**
>
>|          类型           |                   字节流                    |              字符流              |
>| :---------------------: | :-----------------------------------------: | :------------------------------: |
>|       File(文件)        |      FileInputStream、FileOutputStream      |      FileReader、FileWriter      |
>|  Memory Array（数组）   | ByteArrayInputStream、ByteArrayOutputStream | CharArrayReader、CharArrayWriter |
>| Memory String（字符串） |              ----------------               |    StringReader、StringWriter    |
>|      Pipe（管道）       |      PipeInputStream、PipeOutputStream      |      PipeReader、PipeWriter      |





>​                                                                        **处理流**
>
>|                    类型                    |                  字节流                   |             字符流             |
>| :----------------------------------------: | :---------------------------------------: | :----------------------------: |
>|          **Buffering（缓冲流）**           | BufferedInputStream、BufferedOutputStream | BufferedReader、BufferedWriter |
>|            Filtering（过滤流）             |   FilterInputStream、FilterOutputStream   |   FilterReader、FilterWriter   |
>| **Converting between bytes and character** |         ------------------------          |   InputStream、OutputStream    |
>|     Object Serialization(对象序列化流)     |    ObjectInputStream、ObjOutputStream     |      --------------------      |
>|       **Data conversion（数据流）**        |     DataInputStream、DataOutputStream     |    -----------------------     |
>|             Counting（计数流）             |           LineNumberInputStream           |        LineNumberReader        |
>|           Peeking ahead(预读流)            |            PushbackInputStream            |         PushbackReader         |
>|           **Printing（打印流）**           |                PrintStream                |          PrintWriter           |

### 五、InputSteam（输入流）

### 六、OutputSteam（输出流）

### 七、Reader流

### 八、Write流

### 九、节点流讲解

### 十、处理流讲解

