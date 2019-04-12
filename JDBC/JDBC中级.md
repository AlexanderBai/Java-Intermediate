### 一、JDBC的概念及作用

### 二、建立数据库连接

### 三、数据库操作

### 四、JDBC中数据类型

#### 1、基本数据类型

#### 2、日期类型

#### 3、CLOB类型

- CLOB用于存放大量的文本信息

**以下只是为了CLOB类型数据进行操作的示例，代码并不进行优化**

```sql
mysql> create database test;
Query OK, 1 row affected (0.00 sec)
```

```sql
mysql> create table clob_test(id integer primary key,info text);    #info的类型为text
Query OK, 0 rows affected (0.14 sec)
```

```sql
mysql> desc clob_test;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | NO   | PRI | NULL    |       |
| info  | text    | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.11 sec)
```

- 存储CLOB类型数据

  - 读取磁盘上的文件并保存进数据库中

  ```java
  package com.alexanderbai.test;
  
  import org.junit.Test;
  
  import java.io.*;
  import java.sql.*;
  
  /**
   * @Description TODO
   * @Author AlexanderBai
   * @Data 2019/4/9 20:03
   * @Vision 1.0.0
   **/
  public class ClobTest {
      @Test
      public void insertText() {
          Connection connection = null;
          PreparedStatement preparedStatement = null;
          ResultSet resultSet = null;
          
          try {
              Class.forName("com.mysql.jdbc.Driver");    connection=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","ROOT");
          } catch (ClassNotFoundException e) {
              e.printStackTrace();
          } catch (SQLException e) {
              e.printStackTrace();
          }
          
          String sql = "insert into clob_test(id,info)values(?,?)";
          try {
              preparedStatement=connection.prepareStatement(sql);
              preparedStatement.setInt(1,4);
  
          } catch (SQLException e) {
              e.printStackTrace();
          }
          File file = new File("G:\\LoginServlet.java");//指定要读取文件
          FileReader fileReader = null;
          
          try {
              fileReader= new FileReader(file);
          } catch (FileNotFoundException e) {
              e.printStackTrace();
          }
          BufferedReader bufferedReader = new BufferedReader(fileReader);
          try {
              preparedStatement.setCharacterStream(2,bufferedReader,(int)file.length());//保存到数据库中，三个参数分别是索引值、操作的资源文件和操作资源文件的大小
              preparedStatement.execute();
          } catch (SQLException e) {
              e.printStackTrace();
          }
          
          if (resultSet!=null) {
              try {
                  resultSet.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
          if (preparedStatement!=null) {
              try {
                  preparedStatement.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
          if (connection!=null) {
              try {
                  connection.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      }
  }
  ```

- 读取CLOB类型数据

  - 从数据库中读取文件，打印到控制台的同时将文件保存到磁盘上

```java
package com.alexanderbai.test;

import org.junit.Test;

import java.io.*;
import java.sql.*;

/**
 * @Description TODO
 * @Author AlexanderBai
 * @Data 2019/4/9 20:03
 * @Vision 1.0.0
 **/
public class ClobTest {

    @Test
    public void queryText() {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
            connection=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","ROOT");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }
        
        String sql = "select info from clob_test where id=4";
        try {
            preparedStatement=connection.prepareStatement(sql);
            resultSet=preparedStatement.executeQuery();
            while (resultSet.next()) {
                Reader reader=resultSet.getCharacterStream(1);
                BufferedReader bufferedReader=new BufferedReader(reader);

                String temp = "";
                File file = new File("G:\\reader.text");//指定输出的文件
                FileWriter fileWriter=new FileWriter(file);
                BufferedWriter bufferedWriter=new BufferedWriter(fileWriter);

                while ((temp = bufferedReader.readLine()) != null) {
                    System.out.println(temp);//控制台打印
                    bufferedWriter.write(temp+"\n");//添加到磁盘上的文件的末尾
                    bufferedWriter.flush();
                }
                
                bufferedWriter.close();
                fileWriter.close();
                bufferedReader.close();
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }

        if (resultSet!=null) {
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (preparedStatement!=null) {
            try {
                preparedStatement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (connection!=null) {
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

#### 4、BLOB类型

- BLOB是专门针对二进制进行的存取，比如图片、音频、视频等
- 新建数据表

```sql
Create table blob_test(id integer primary key ,info blob);	     #info类型为BLOB类型数据
```

```sql
+----------------+
| Tables_in_test |
+----------------+
| blob_test      |
| clob_test      |
+----------------+
2 rows in set (0.00 sec)
```

```sql
mysql> desc blob_test;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| id    | int(11) | NO   | PRI | NULL    |       |
| info  | blob    | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.01 sec)
```

- 存储BLOB类型数据

  ```java
  package com.alexanderbai.test;
  
  import org.junit.Test;
  
  import java.io.*;
  import java.sql.*;
  
  /**
   * @Description TODO
   * @Author AlexanderBai
   * @Data 2019/4/9 20:03
   * @Vision 1.0.0
   **/
  public class BlobTest {
      
      @Test
      public void insertBlob() {
          Connection connection = null;
          PreparedStatement preparedStatement = null;
          ResultSet resultSet = null;
          
          try {
              Class.forName("com.mysql.jdbc.Driver");          connection=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","ROOT");
          } catch (ClassNotFoundException e) {
              e.printStackTrace();
          } catch (SQLException e) {
              e.printStackTrace();
          }
          
          String sql = "insert into blob_test(id,info)values(?,?)";
          try {
              preparedStatement=connection.prepareStatement(sql);
              preparedStatement.setInt(1,3);
  
          } catch (SQLException e) {
              e.printStackTrace();
          }
          
          //从磁盘加载文件
          File file = new File("C:\\Users\\AlexanderBai\\Pictures\\images\\3.jpg");
          FileInputStream fileInputStream = null;
          try {
              fileInputStream=new FileInputStream(file);
          } catch (FileNotFoundException e) {
              e.printStackTrace();
          }
          try {
              //图片、音频、视频等都是以二进制的形式进行
              preparedStatement.setBinaryStream(2,fileInputStream,(int)file.length());
              preparedStatement.execute();
              fileInputStream.close();
          } catch (SQLException e) {
              e.printStackTrace();
          } catch (IOException e) {
              e.printStackTrace();
          }
          
          if (resultSet!=null) {
              try {
                  resultSet.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
          if (preparedStatement!=null) {
              try {
                  preparedStatement.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
          if (connection!=null) {
              try {
                  connection.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      }
  }
  ```

- 读取BLOB类型数据

  ```java
  package com.alexanderbai.test;
  
  import org.junit.Test;
  
  import java.io.*;
  import java.sql.*;
  
  /**
   * @Description TODO
   * @Author AlexanderBai
   * @Data 2019/4/9 20:03
   * @Vision 1.0.0
   **/
  public class BlobTest {
      
      @Test
      public void queryBlob() {
          Connection connection = null;
          PreparedStatement preparedStatement = null;
          ResultSet resultSet = null;
          
          try {
              Class.forName("com.mysql.jdbc.Driver");          connection=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","ROOT");
          } catch (ClassNotFoundException e) {
              e.printStackTrace();
          } catch (SQLException e) {
              e.printStackTrace();
          }
  		
          //查询 info 字段信息
          String sql = "select info from blob_test where id=3";
          try {
              preparedStatement=connection.prepareStatement(sql);
              resultSet=preparedStatement.executeQuery();
              
              while (resultSet.next()) {
                  //获取二进制流形式，并指定输出流
                  InputStream inputStream=resultSet.getBinaryStream(1);
                  //指定输出流目标文件，这一步会在相应的路径下新建一个jpg文件，此时的文件为空
                  File file = new File("G:\\1.jpg");
                  OutputStream outputStream = new FileOutputStream(file);
                  int len = 0;
                  byte[] bytes = new byte[1024];
                  while ((len = inputStream.read(bytes)) > 0) {
                      //把从数据库中读取的文件写入到磁盘上的jpg文件
                      outputStream.write(bytes,0,len);
                      System.out.println((char)len);//在控制台测试，打印的是字符
                  }
  
                  outputStream.flush();
                  outputStream.close();
                  inputStream.close();
              }
          } catch (SQLException e) {
              e.printStackTrace();
          } catch (IOException e) {
              e.printStackTrace();
          }
  
          if (resultSet!=null) {
              try {
                  resultSet.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
          if (preparedStatement!=null) {
              try {
                  preparedStatement.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
          if (connection!=null) {
              try {
                  connection.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      }
  }
  ```

#### 五、DAO设计模式

### 六、JDBC对事务的支持

