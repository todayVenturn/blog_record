# 课前准备

## 开篇词 | 学习准备

在实战中学习，是解锁MySQL技能的最佳方法

**为什么学了很多知识，你依然不会用 MySQL 解决实际问题?**

>     没有形成一个闭环，只是学习了零碎的知识点，没有在项目中运用到，简单说就是没有情景。
>
>   **在工作中，最重要的绝对不是你的知识储备量，而是你解决实际问题的能力。**



### 这门课的目录：

-   **实践篇：**从基础出发； 包括创建数据库、数据表、对表中的数据进行增删改查操作、使用函数、表与表之间的关联操作等，快速掌握最基本的用法。 
-   **进阶篇：** MySQL 的许多高级功能。  包括把程序存储在服务器上、利用突发事件来调用程序、在不改变存储结构的前提下创建虚拟表以方便查询，等等。 
-   **优化篇：**性能优化和数据库的设计规范，理清设计思路。
-   **案例篇：**从0到1设计一个连锁超市的信息系统数据库



## 环境准备 | 安装MySQL和图形管理工具

### 安装与配置

下载MySQL的安装包：

1.  [MySQL开发者专区](https://dev.mysql.com)

2.  安装MySQL数据库服务器及相关组件

>   MySQL  Server：是 MySQL 数据库服务器，这是 MySQL 的核心组件。
>
>   MySQL  Workbench：是一个管理 MySQL 的图形工具，一会儿我还会带你安装它。
>
>   MySQL  Shell：是一个命令行工具。除了支持 SQL 语句，它还支持 JavaScript 和 Python 脚本，并且支持调用 MySQL API 接口。
>
>   MySQL  Router：是一个轻量级的插件，可以在应用和数据库服务器之间，起到路由和负载均衡的作用。听起来有点复杂，我们来想象一个场景：假设你有多个 MySQL 数据库服务器，而前端的应用同时产生了很多数据库访问请求，这时，MySQL  Router 就可以对这些请求进行调度，把访问均衡地分配给每个数据库服务器，而不是集中在一个或几个数据库服务器上。
>
>   Connector/ODBC：是 MySQL 数据库的 ODBC 驱动程序。ODBC 是微软的一套数据库连接标准，微软的产品（比如 Excel）就可以通过 ODBC 驱动与 MySQL 数据库连接。

3.  配置服务器

>   配置服务器的类型：开发计算机（最小的内存）、服务器计算机（中等程度）和专属计算机（全部内存资源）。区别在于，MySQL数据库服务器会占用多大的内存。

4.  身份验证配置
5.  设置密码和用户权限
6.  配置windows服务



### 图形化管理工具

可以选择MySQL自带的workbench，不过它是英文版的。也可以使用SQLyog。



# 实践篇

## 01 | 存储：一个完整的数据存储过程

 在 MySQL 中，一个完整的**数据存储过程总共有 4 步，分别是创建数据库、确认字段、创建数据表、插入数据。** 

![1645696082493](MySQL必知必会.assets/1645696082493.png)

### 第一：创建MySQL数据库

1.  **如何创建爱你数据库：**

```
create database demo;
```

2.  如何查看数据库：

```
mysql> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| demo               |
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)
```

为什么会有其他的数据库？

-   “information_schema”是 MySQL 系统自带的数据库，主要保存 MySQL 数据库服务器的**系统信息，比如数据库的名称、数据表的名称、字段名称、存取权限、数据文件所在的文件夹和系统使用的文件夹，**等等。

-   “performance_schema”是 MySQL 系统自带的数据库，可以用来**监控 MySQL 的各类性能指标。**

-   “sys”数据库是 MySQL 系统自带的数据库，主要作用是，以一种更容易被理解的方式展示 MySQL 数据库服务器的各类性能指标，帮助系统管理员和开发人员监控 MySQL 的技术性能。

-   “mysql”数据库保存了 MySQL 数据库服务器运行时需要的系统信息，比如数据文件夹、当前使用的字符集、约束检查信息，等等。



### 第二：确认表的字段

​	MySQL 数据表由行与列组成，一行就是**一条数据记录**，每一条数据记录都被分成许多列，一列就叫**一个字段**。每个字段都需要定义数据类型，这个数据类型叫做字段类型。 



### 第三：创建数据表

可以借助使用图形工具帮我们创建。也可以使用SQL语句，自己创建。

```
CREATE TABLE demo.test
( 
  barcode text,
  goodsname text,
  price int
); 
```

**创建表的时候，最好指明数据库**。否则，如果你没有选中数据库，Workbench 会提示错误；要是你当前选中的数据库不对，还可能把表创建到错误的数据库中。

**不要在最后一个字段的后面加逗号“,”，这也是初学者容易犯的错误**。

同时也要注意起的名字不能与SQL中保留关键词同名，如果同名需要用``引起来。



#### 查看表的结构

```
DESCRIBE demo.test;
```

运行结构如下：

```

mysql> DESCRIBE demo.test;
+-----------+------+------+-----+---------+-------+
| Field     | Type | Null | Key | Default | Extra |
+-----------+------+------+-----+---------+-------+
| barcode   | text | YES  |     | NULL    |       |
| goodsname | text | YES  |     | NULL    |       |
| price     | int  | YES  |     | NULL    |       |
+-----------+------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

>   Field：表示字段名称。
>
>   Type：表示字段类型，这里 barcode、goodsname 是文本型的，price 是整数类型的。
>
>   Null：表示这个字段是否允许是空值（NULL）。这里你一定要注意，在 MySQL 里面，空值不等于空字符串。一个空字符串的长度是 0，而一个空值的长度是空。而且，在 MySQL 里面，空值是占用空间的。
>
>   Key：我们暂时把它叫做键。
>
>   Default：表示默认值。我们导入的表的所有的字段都允许是空，默认值都是 NULL。
>
>   Extra：表示附加信息。



#### 查看数据库中的表

先用USE语句选中数据库：

```
use demo;
```

运行后，进入demo数据库，然后用show语句查询这个数据库中的所有表：

```
show tables;
```



#### 如何设置主键？

**主键可以确保数据的==唯一性==，而且能够减少数据错误。**

在创建数据表的时候一定要有主键！！！

-   非空
-   必须唯一，不能重复
-   必须可以唯一标识数据表中的记录



>   可以有两种方式设置主键，第一种是使用图形界面：SQLyog等；第二种是用SQL语句

```SQL
ALTER TABLE demo.test
ADD COLUMN itemnumber int PRIMARY KEY AUTO_INCREMENT;
```

-   ALTER TABLE，表示修改表；

-   ADD COLUMN，表示增加一列；

-   PRIMARY KEY，表示这一列是主键；

-   AUTO_INCREMENT，表示每增加一条记录，这个值自动增加。一会儿讲到添加数据的时候，我还会详细介绍一下它。



### 第四：插入数据

可以通过数据导入命令，大批量的进行数据写入；也可以使用`insert`语句插入一条记录。

```sql
INSERT INTO demo.test
(barcode,goodsname,price)
VALUES ('0001','本',3);
```

 这里的 INSERT INTO 表示向 demo.test 中插入数据，后面是要插入数据的字段名，VALUES 表示对应的值。 

有两点需要格外注意：

1.  要插入的字段名可以不写，但是为了**可读性**，最好都写上
2.   由于字段 itemnumber 定义了 AUTO_INCREMENT，所以我们插入一条记录的时候，不给它赋值，系统也会自动给它赋值。 



## 总结：

这一讲主要是是学习了数据存储的完整过程。包括**创建数据库、创建数据表、确定字段和插入数据**。

```sql
-- 创建数据库
CREATE DATABASE demo；
-- 删除数据库
DROP DATABASE demo；
-- 查看数据库
SHOW DATABASES;
-- 创建数据表：
CREATE TABLE demo.test
(  
  barcode text,
  goodsname text,
  price int
); 
-- 查看表结构
DESCRIBE demo.test;
-- 查看所有表
SHOW TABLES;
-- 添加主键
ALTER TABLE demo.test
ADD COLUMN itemnumber int PRIMARY KEY AUTO_INCREMENT;
-- 向表中添加数据
INSERT INTO demo.test
(barcode,goodsname,price)
VALUES ('0001','本',3);
```



## 02 | 字段：如何定义字段？

### 整数类型

一共有五中，包括TINYINT、SMALLINT、MEDIUMINT、INT（INTEGER）和 BIGINT 。

**如何选择？**

其实评估选择那种整数类型的时候，就是在**考虑存储空间和可靠性的平衡问题**；一方面用小容量的数据类型可以节省空间；另一方面，要是为了节省空间，使用的数据类型范围太小了，一旦超出取值范围，就可能引起系统错误，影响可靠性。



首线要确保**数据不会超过取值范围**，在这个前提下载去考虑节省存储空间。



### 浮点数类型和定点数类型

它们都可以处理小数。浮点类型分别是： FLOAT、DOUBLE、REAL 。

-   FLOAT 表示单精度浮点数；

-   DOUBLE 表示双精度浮点数；

-   REAL 默认就是 DOUBLE。如果你把 SQL 模式设定为启用“REAL_AS_FLOAT”，那么，MySQL 就认为 REAL 是 FLOAT。如果要启用“REAL_AS_FLOAT”，就可以通过以下 SQL 语句实现：

```sql
SET sql_mode = “REAL_AS_FLOAT”;
```

浮点数类型有个缺陷，**不精准**。因此，在一些对精确度要求比较高的项目中，不能使用浮点数。

**为什么会不精准？**

>   原因在MySQL对浮点类型数据的存储方式。

MySQL存储数据的方式参考**计算机组成原理**

[IEEE754标准](https://blog.csdn.net/gao_zhennan/article/details/120717424)

[MySQL定点数详解](https://blog.csdn.net/gao_zhennan/article/details/120717424)

 因为在小数从十进制转换为二进制可能会丢失精度，所以就不转了，**直接用小数点两边的两个整数搭配就可以了** 。因为整数从十进制转换为二进制不会丢失精度，所以这就确保了这种方式不会丢失精度。 



**定点数类型**

 MySQL 用 DECIMAL（M,D）的方式表示高精度小数。其中，M 表示整数部分加小数部分，一共有多少位，M<=65。D 表示小数部分位数，D<M。 



**总结一下：**浮点类型取值范围大，但是不精准，适用于需要取值范围大，又可以容忍微小误差的科学计算场景（比如计算化学、分子建模等）；定点数类型取值范围相对小，但是精准，没有误差，适合用对精度要求极高的场景（比如金融计算）



### 文本类型

TEXT 类型是 MySQL 支持的文本类型的一种。此外，MySQL 还支持 CHAR、VARCHAR、ENUM 和 SET 等文本类型。 

>   CHAR(M)：固定长度字符串。CHAR(M) 类型必须预先定义字符串长度。如果太短，数据可能会超出范围；如果太长，又浪费存储空间。 

>    VARCHAR(M)： 可变长度字符串。VARCHAR(M) 也需要预先知道字符串的最大长度，不过只要不超过这个最大长度，具体存储的时候，是按照实际字符串长度存储的。 

>    TEXT：字符串。系统自动按照实际长度存储，不需要预先定义长度。 

>    ENUM： 枚举类型，取值必须是预先设定的一组字符串值范围之内的一个，必须要知道字符串所有可能的取值。 

>    SET：是一个字符串对象，取值必须是在预先设定的字符串值范围之内的 0 个或多个，也必须知道字符串所有可能的取值。 

 对于 ENUM 类型和 SET 类型来说，你必须知道所有可能的取值，所以只能用在某些特定场合，比如某个参数设定的取值范围只有几个固定值的场景。 

 因为**不需要预先知道字符串的长度**，系统会按照实际的数据长度进行存储，所以 TEXT 类型最为灵活方便。



TEXT 类型也有 4 种，它们的区别就是最大长度不同。

-   TINYTEXT：255 字符（这里假设字符是 ASCII 码，一个字符占用一个字节，下同）。

-   TEXT： 65535 字符。

-   MEDIUMTEXT：16777215 字符。

-   LONGTEXT： 4294967295 字符（相当于 4GB）。

 **由于实际存储的长度不确定，MySQL 不允许 TEXT 类型的字段做主键。遇到这种情况，你只能采用 CHAR(M)，或者 VARCHAR(M)。** 

所以，只要不是主键字段，就可以按照数据可能的最大长度，选择这几种TEXT类型中的一种，作为存储字符串的数据类型。



### 日期与时间类型

 **用得最多的日期时间类型，就是 DATETIME**。虽然 MySQL 也支持 YEAR（年）、TIME（时间）、DATE（日期），以及 TIMESTAMP 类型，但是**我建议你，在实际项目中，尽量用 DATETIME 类型**。 

>    另外，这里还有个问题，为什么时间类型 TIME 的取值范围不是 -23:59:59～23:59:59 呢？原因是 MySQL 设计的 TIME 类型，不光表示一天之内的时间，而且可以用来表示**一个时间间隔**，这个时间间隔可以超过 24 小时。 



## 总结：

在定义数据类型时，如果确定是整数，就用`int`；如果是小数，一定用定点数类型`decimal`；如果是字符串，只要不是主键，就用`text`；如果是日期与时间，就用`datetime`

-   整数：INT。

-   小数：DECIMAL。

-   字符串：TEXT。

-   日期与时间：DATETIME。



## 03 | 怎么创建和修改表？

**MySQL创建表的语法结构：**

```sql
CREATE TABLE <表名>
{
    字段名1 数据类型 [字段级别约束] [默认值]，
    字段名2 数据类型 [字段级别约束] [默认值]，
    ......
    [表级别约束]
};
```

但是注意，我们可能平时不太常用SQL语句创建数据表；更多的是通过SQLyog这样的图形化软件辅助我们创建表。



**查询表的内容：**

```sql
SELECT *
FROM demo.importhead
```



#### 约束条件：

**1.非空约束：**

 	非空约束表示字段值不能为空，如果创建表的时候，指明某个字段非空，那么添加数据的时候，这个字段必须有值，否则系统就会提示错误。 

**2. 唯一性约束**

唯一性约束表示这个字段的值不能重复，否则系统会提示错误。跟主键约束相比，唯一性约束要更加弱一些。

 在一个表中，我们可以指定多个字段满足唯一性约束，而主键约束则只能有一个，这也是 MySQL 系统决定的。另外，**满足主键约束的字段，自动满足非空约束，但是满足唯一性约束的字段，则可以是空值**。 

**3. 自增约束**

自增约束可以让 MySQL 自动给字段赋值，且保证不会重复。

但是使用自增约束的时候需要注意两个问题：

​	第一，只有整数类型才可以定义自增约束

​	第二，可以给自增约束的字段复制，这样MySQL会重置自增字段的自增基数，下次添加数据时， 自动以自增约束字段的最大值加 1 为新的字段值。 



#### 修改表

一般我们都会去复制表结构，然后在这个基础上通过修改表结构来创建新的表

**把原来的表结构复制一下**

```sql
CREATE TABLE demo.importheadhist
LIKE demo.importhead;
```

 运行这个语句之后，一个跟 demo.importhead 有相同表结构的空表 demo.importheadhist，就被创建出来了。 

**添加字段**

可以使用SQLyog或者workbench，用可视化操作来修改表的结构；也可以使用SQL语句。

给这个新的表增加 2 个字段：confirmer 和 confirmdate 

```sql
ALTER TABLE demo.importheadhist
ADD confirmer INT; -- 添加一个字段confirmer，类型INT

ALTER TABLE demo.importheadhist
ADD confirmdate DATETIME; -- 添加一个字段confirmdate，类型是DATETIME
```

**修改字段**

 通过修改表结构语句`ALTER TABLE` 

```sql
ALTER TABLE demo.importheadhist
CHANGE quantity importquantity DOUBLE;
```

 把字段名称“quantity”改成“importquantity”，并且把字段类型改为 DOUBLE 

如果不想改字段名字，只想改字段类型。例如，把字段“importquantity”类型改成 DECIMAL(10,3) 

```sql
ALTER TABLE demo.importheadhist
MODIFY importquantity DECIMAL(10,3);
```

**向表中添加一个字段，并且指定添加字段在表中的位置**

```sql
ALTER TABLE demo.importheadhist
ADD suppliername TEXT AFTER supplierid;
```

 在字段 supplierid 之后，添加一个字段 suppliername，数据类型是 TEXT。 



## 总结：

这一节，我们学习了创建表和修改表的具体方法。并且还学习了**约束**

-   默认值约束：给字段设置一个默认值
-   非空约束：声明字段不能为空值
-   唯一性约束：声明字段不能重复
-   自增约束：声明字段值能够自动加1，且不会重复

```sql
CREATE TABLE    --主键约束
(
	字段名 字段类型 PRIMARY KEY
); 
CREATE TABLE   -- 非空约束
(
	字段名 字段类型 NOT NULL
);
CREATE TABLE   -- 唯一约束
(
	字段名 字段类型 UNIQUE
);
CREATE TABLE    -- 默认值约束
(
	字段名 字段类型 DEFAULT 值
);
-- 这里要注意自增类型的条件，字段类型必须是整数类型。
CREATE TABLE
(
	字段名 字段类型 AUTO_INCREMENT
);

-- 在一个已经存在的表基础上，创建一个新表
CREATE demo.importheadhist LIKE demo.importhead;

-- 修改表的相关语句
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 数据类型;
ALTER TABLE 表名 ADD COLUMN 字段名 字段类型 FIRST|AFTER 字段名;
ALTER TABLE 表名 MODIFY 字段名 字段类型 FIRST|AFTER 字段名;
```

