### 一、类集

####1、集合的作用

- **集合和对象数组类似，都是保存对象的容器。但是集合更加强大，提供了添加、遍历、排序和删除等一系列基本操作。且集合可以动态扩容。**

- **在实际开发中，大多数情况下数据的测定依赖于对象的传递，只有极少数是在操作具体的数据（联系DAO开发模式）**

  

#### 2、操作单值对象的集合-Collection家装

- **以下的实现类不是直接实现接口的，省略了一部分抽象类（Map家族也是）**

![1553519902773](assets/1553519902773.png)

- **List：**
  - 有序，元素可重复
  - 允许为null
- **Set：**
  - 无序,不可重复
  - 底层大多是Map结构的实现
  - 常用的三个实现类都是线程非同步

####3、操作二元偶对象的集合-Map家族



![1553519880967](assets/1553519880967.png)

- 存储结构是key-value键值对，不像Collection是单列集合
- 必须知道什么是散列表和红黑树

### 二、Collection接口

- **Collection接口提供了一系列操作集合的方法**

![1553502047404](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1553502047404.png)

### 三、List接口

- **list接口继承了Collection接口**

![1553501796778](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1553501796778.png)

### 四、Set接口

- **Set接口继承了Collection接口**

![1553501972995](C:\Users\AlexanderBai\AppData\Roaming\Typora\typora-user-images\1553501972995.png)

### 五、SortedSet接口



### 六、集合的输出

#### 1、Iterator迭代输出



####2、foreach输出



###七、Map接口



### 八、SortedMap接口



### 九、其他集合类

#### 1、Stack类



#### 2、属性类Properties

- Properties一般用于配置文件，以key-value的结构存储

- 如常用的有关`jdbc`连接数据库的配置

  

### 十、demo

#### 1、一对多关系



####2、多对多关系