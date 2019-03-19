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
- **在开发中输入输出是站在程序的角度而言**

### 四、节点流与数据流

####1、节点流与处理流的关系

> - **节点流：**指**直接**对指定的数据源进行操作
> - **处理流：**是指连接在**已有的流（节点流或数据流）**之上，为程序提供更为强大的读写功能的流。这里用**连接来**形容流是因为数据源与程序之间必须先建立一个用于数据传输的通道才能进行数据的传输（**如：HTTP连接**）
> - ![1552826167246](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552826167246.png)

####2、节点流						

>​                                                                            **节点流**
>
>|          类型           |                   字节流                    |              字符流              |
>| :---------------------: | :-----------------------------------------: | :------------------------------: |
>|     **File(文件)**      |    **FileInputStream、FileOutputStream**    |    **FileReader、FileWriter**    |
>|  Memory Array（数组）   | ByteArrayInputStream、ByteArrayOutputStream | CharArrayReader、CharArrayWriter |
>| Memory String（字符串） |              ----------------               |    StringReader、StringWriter    |
>|      Pipe（管道）       |      PipeInputStream、PipeOutputStream      |      PipeReader、PipeWriter      |



####3、处理流

>​                                                                        **处理流**
>
>|                    类型                    |                    字节流                     |               字符流               |
>| :----------------------------------------: | :-------------------------------------------: | :--------------------------------: |
>|          **Buffering（缓冲流）**           | **BufferedInputStream、BufferedOutputStream** | **BufferedReader、BufferedWriter** |
>|            Filtering（过滤流）             |     FilterInputStream、FilterOutputStream     |     FilterReader、FilterWriter     |
>| **Converting between bytes and character** |           ------------------------            |     InputStream、OutputStream      |
>|     Object Serialization(对象序列化流)     |      ObjectInputStream、ObjOutputStream       |        --------------------        |
>|       **Data conversion（数据流）**        |       DataInputStream、DataOutputStream       |      -----------------------       |
>|             Counting（计数流）             |             LineNumberInputStream             |          LineNumberReader          |
>|           Peeking ahead(预读流)            |              PushbackInputStream              |           PushbackReader           |
>|           **Printing（打印流）**           |                  PrintStream                  |            PrintWriter             |

### 五、InputSteam（输入流）

####1、InputStream相关流

> - 继承自InputStream的流都是一字节（8位）为单位进行流的输入
> - 深色为节点流
>
> ![1552922903338](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552922903338.png)

#### 2、InputStr常用方法

![1553005288438](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1553005288438.png)

### 六、OutputSteam（输出流）

####1、OutputStream相关流

> - 与OutputStream相关的流都是以字节为单位进行写操作
> - 深色为节点流
>
> ![1552923263829](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552923263829.png)

#### 2、OutputStream常用方法

![1553005742022](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1553005742022.png)

### 七、Reader流

####1、与Reader相关的流

> - 与Reader相关的流都是以字符（16位）我单位进行读操作
>
> - 深色为节点流
>
>   ![1552972215241](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552972215241.png)

#### 2、Reader常用方法

![1553006082799](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1553006082799.png)

### 八、Write流

#### 1、与Writer有关的流

> - 与Writer有关的流都是以字符为单位进行写操作
>
> - 深色为节点流
>
>   ![1552972573850](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552972573850.png)

#### 2、Writer 常用方法

![1553006491083](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1553006491083.png)

### 九、节点流讲解

####1、以File类型做为节点流的典型代表

> - **有关文件读写的常用构造方法**
>
> | FilterInputStream(File file)     | FileReader(File file)       |
> | -------------------------------- | --------------------------- |
> | FilterOutputStream(String namen) | FileWriter(String fileName) |
>
> - **`FileInputStream`的常用方法**
>
>   | Modifier | Method and Description                                       |
>   | :------: | ------------------------------------------------------------ |
>   |   int    | read() ： 从该输入流读取下一个数据字节                       |
>   |   int    | read(byte[] b):从该输入流读取最多byte.length个字节的数据到字节数组 |
>   |   int    | read(byte[] b,int off,int len)：从该输入流读取最多len个字节的数据到字节数组 |



####2、使用FileInputStream进行文件读操作

>```java
>package Demo;
>
>import java.io.FileInputStream;
>import java.io.FileNotFoundException;
>import java.io.IOException;
>
>/**
> * @Author AlexanderBai
> * @data 2019/3/18 9:24
> */
>public class TestFileInputStream {
>    public static void main(String[] args) {
>        int b=0;//用b来装掉用read()方法返回时的整数
>        int num=0;//用num来记录读取的字节数
>        FileInputStream fileInputStream=null;
>        try {
>            fileInputStream = new FileInputStream("G:\\Demo.java");
>        } catch (FileNotFoundException e) {
>            System.out.println("系统找不到指定的文件!!!");
>            System.exit(-1);//系统非正常退出
>        }
>        try {
>            while ((b=fileInputStream.read()) != -1) {
>                System.out.print((char)b);//把使用数字表示的汉字和字符转换成字符输出
>                num++;
>            }
>            fileInputStream.close();
>            System.out.println();
>            System.out.println("总共读取了"+num+"字节");
>        } catch (IOException e) {
>            System.out.println("文件读取错误！！！");
>        }
>    }
>}
>```
>
>- **观察上面的运行结果，可以发现中文部分出现乱码，是因为文件以字节流的形式读取，而在Java中中文占两个字节，故出现乱码**

#### 3、使用FileOutputStream对文件进行写操作

> - 对文件进行复制
>
>   ```java
>   package Demo;
>   
>   import java.io.FileInputStream;
>   import java.io.FileNotFoundException;
>   import java.io.FileOutputStream;
>   import java.io.IOException;
>   
>   /**
>    * @Author AlexanderBai
>    * @data 2019/3/18 10:39
>    */
>   public class TestFileOutputStream {
>       public static void main(String[] args) {
>           int b=0;//用b来装掉用read()方法返回时的整数
>           int num=0;//用num来记录读写的字节数
>           FileInputStream fileInputStream=null;
>           FileOutputStream fileOutputStream=null;
>           try {
>               fileInputStream = new FileInputStream("G:\\Demo.java");
>               fileOutputStream= new FileOutputStream("G:\\Test.java");//直接在盘符上写，系统自动创建
>               /*
>               *fileOutputStream= new FileOutputStream("G:\\Demo\\Test.java");
>               *不是直接在盘符上进行的写操作，首先要确认目录是否存在，
>               * 若不存在则会抛出FileNotFoundException
>               **/
>   
>           } catch (FileNotFoundException e) {
>               System.out.println("系统找不到指定的文件!!!");
>               System.exit(-1);//系统非正常退出
>           }
>           try {
>               while ((b=fileInputStream.read() )!= -1) {
>                       fileOutputStream.write(b);
>                   System.out.print((char)b);
>                   num++;
>               }
>               fileInputStream.close();
>               System.out.println();
>               System.out.println("总共写入了"+num+"字节");
>               System.out.println("Demo.java文件里面的内容已经成功复制到文件Test.java里面");
>           } catch (IOException e) {
>               System.out.println("文件复制失败！");
>           }
>   
>       }
>   }
>   ```
>
> - 上面的程序除了对文件进行读写之外，还在控制台打印，通过观察可以发现控制台上的中文乱码，而文件Test.java打开之后中文并没有乱码，**可能**是记事本做了优化，但`FileOutputStream`依然是以字节为单位进行写操作

### 4、使用字符流对文件进行读写

>- 对文件进行复制
>
>  ```java
>  package Demo;
>  
>  import java.io.*;
>  
>  /**
>   * @Author AlexanderBai
>   * @data 2019/3/18 21:09
>   */
>  public class TestFilelWriter {
>      public static void main(String[] args) {
>          File file = new File("");
>          int b=0;
>          FileReader fileReader=null;
>          FileWriter fileWriter=null;
>          try {
>              fileReader = new FileReader("G:" + File.separator + "Demo.java");
>              fileWriter = new FileWriter("G:" + File.separator + "demo.txt");
>    //若不存在，系统自动创建，且必须是直接在盘符上，负责抛出FileNotFoundException异常
>  /*如：fileWriter = new FileWriter("G:" + File.separator +"d"+File.separator +"demo.txt");（这种写法必须保证目录d存在）*/
>              while ((b=fileReader.read())!= -1) {
>                  fileWriter.append((char)b);
>              }
>              //对文件进行操作是必须关闭资源，不然可能看不到写出的文件，也可以调用刷新方法
>              //fileWriter.flush();
>              fileWriter.close();
>          } catch (FileNotFoundException e) {
>              e.printStackTrace();
>          } catch (IOException e) {
>              e.printStackTrace();
>          }
>      }
>  }
>  ```
>
>- **对文件进行操作是必须关闭资源，不然可能看不到写出的文件，也可以调用刷新方法**



### 十、处理流讲解

#### 1、缓冲流

![1552973777091](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1552973777091.png)

> ```java
> package Demo;
> 
> import java.io.*;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/19 13:39
>  */
> public class TestBufferStream {
>     public static void main(String[] args) {
>         int b=0;
>         FileInputStream fileInputStream =null;
>         BufferedInputStream bufferedInputStream=null;
>         try {
>             //在FileInputStream上套接一个缓冲
>             fileInputStream = new FileInputStream("G:" + File.separator + "Demo.java");
>             bufferedInputStream = new BufferedInputStream(fileInputStream);
>             bufferedInputStream.mark(100);//在地100个字符处做一个标记
> 
>             for (int i = 0; i < 50&&(b=bufferedInputStream.read())!=-1; i++) {
>                System.out.print((char) b);
>             }
> 
>             System.out.println("--------------------------------------------------------------------");
>             //重新读取
>             bufferedInputStream.reset();
>             try {
>                 while ((b=bufferedInputStream.read() )!= -1) {
>                     System.out.print((char) b);
>                 }
>             } catch (IOException e) {
>                 e.printStackTrace();
>             }
>         } catch (FileNotFoundException e) {
>             e.printStackTrace();
>         } catch (IOException e) {
>             e.printStackTrace();
>         }
>     }
> 
> }
> ```
>
> - **mark(int readlimit):**类似于书签，用于标记，为reset掉用做左后标记
>
>   ```java
>    // @param   readlimit   the maximum limit of bytes that can be read before
>    //                    the mark position becomes invalid.
>   ```
>
> - **reset():**调用此方法之后可以重新读取读过的数据
>
>   ```java
>   /** If <code>markpos</code> is <code>-1</code>
>   * (no mark has been set or the mark has been
>   * invalidated), an <code>IOException</code>
>   * is thrown. Otherwise, <code>pos</code> is
>   * set equal to <code>markpos</code>.
>   ***/
>   ```
>
> - 官方说是若读取的数据超过了mark标记的位置，则调用reset方法后无效。但在实际的开发中可以看出，当缓冲区已满溢出时，调用reset方法才无效。所以实际上reset是否有效取决于缓冲区的状态，并不是readlimit的值

> - **BufferedWriter和BufferedReader**测试
>
> ```java
> package Demo;
> 
> import java.io.*;
> 
> /**
>  * @Author AlexanderBai
>  * @data 2019/3/19 17:03
>  */
> public class TestBufferStream1 {
>     public static void main(String[] args) {
>         int b;
>         String string=null;
>         String string1=null;
>         FileWriter fileWriter=null;
>         FileReader fileReader = null;
>         BufferedWriter bufferedWriter=null;
>         BufferedReader bufferedReader=null;
> 
>         try {
>             fileWriter = new FileWriter("G:" + File.separator + "new.txt");
>             bufferedWriter = new BufferedWriter(fileWriter);
> 
>             for (int i = 0; i < 500; i++) {
>                 string = String.valueOf(Math.random());
>                 bufferedWriter.write(string);
>                 bufferedWriter.newLine();
>                 System.out.println(string);
>             }
>             bufferedWriter.flush();//必须对缓冲流进行刷新，不然报错
> 
>             fileReader = new FileReader("G:" + File.separator + "new.txt");
>             bufferedReader = new BufferedReader(fileReader);
>             while ((string1=bufferedReader.readLine())!=null) {
>                 System.out.println(string1);
>             }
>             //按顺序进行关闭
>             fileWriter.close();
>             bufferedWriter.close();
>             fileReader.close();
>             bufferedReader.close();
>         } catch (IOException e) {
>             e.printStackTrace();
>         }
>     }
> }
> ```

#### 2、转换流

####3、数据流

####4、打印流

####5、对象流



