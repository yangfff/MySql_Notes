##  〇、数据库学习阶段：

1. 基础阶段：mysql数据库的基本操作（增删改查），以及一些高级操作（视图，触发器，函数，存储过程等）
2. 优化阶段：提高数据库效率，如索引，分表等 
3. 部署阶段：搭建真实的环境系统，如服务器集群，负载均衡。

本文为初级阶段的完整学习笔记，学自CSDN视频课[《6天玩转MySQL》](https://edu.csdn.net/course/detail/2300?ops_request_misc=%7B%22request%5Fid%22%3A%22158174139519725222402725%22%2C%22scm%22%3A%2220140713.130063507..%22%7D&request_id=158174139519725222402725&biz_id=3&utm_source=distribute.pc_search_result.none-task)，非常感谢平台与作者！

学习时间：2020年2月15日一天

## 一、数据库基础

1、什么是数据库？

- 数据库：database，存储数据的仓库

- 数据库：高效的存储和处理数据的介质（介质主要是两种：磁盘和内存）



2、数据库的分类

- 数据库基于存储介质的不同，进行分类：关系型数据库（SQL）和非关系型数据库（NoSQL：Not Only SQL，不是关系型的数据库都叫做非关系型数据库）。



3、不同的数据库阵营中的产品有哪些？

- 关系型数据库

大型：Oracle,DB2

中型：SQL-SERVER，Mysql等

小型：access



- 非关系型数据库

memcached,mongodb,redis(同步到磁盘)



4、两种数据库阵营的区别？

- 关系型数据库：安全（保存磁盘基本不可能丢失），容易理解，比较浪费空间（二维表）



- 非关系型数据库：效率高，不安全（断电丢失）


### 1.1 关系型数据库



1、什么是关系型数据库？

关系型数据库：是一种建立在关系模型（数学模型）上的数据库。



关系模型：一种所谓建立在关系上的模型，关系模型包含三个方面：

- 数据结构：数据存储结果，二维表（有行和列）

- 操作指令集合：所有的SQL语句

- 完整性约束：表内数据约束（字段与字段），表与表之间的约束（外界）



2、关系型数据库的设计？

关系型数据库：从需要存储的数据需求中分析，如果是一类数据（实体）应该设计成一张二维表，表是由表头（字段名：用来规定数据的名字）和数据部分组成（实际存储的数据单元）



-二维表：行和列

可参考案例“教学系统”



-关系型数据库：维护实体内部，实体与实体之间的联系。其特点之一：如果表中对应的某个字段没有值（数据），但是系统依然要分配空间：关系型数据库比较浪费空间。
 

### 1.2 关键字说明



数据库：database

数据库系统：DBS（Database System）是一种虚拟系统，将多种内容关联起来的称呼

DBS = DBMS + DB

DBMS：Database Management System，数据库管理系统，专门管理数据库

DBA：Database Administrator，数据库管理员



行/记录：row / record ，本质上一个东西：都是指表中的一行（一条记录）:行是从结构角度出发，记录是从数据角度出发。

列/字段：column/field，本质是同一个东西。

### 1.3 SQL



SQL：Structured Query Language，结构化查询语言（数据以查询为主：99%是在进行查询操作）



SQL分为三个部分：
- DDL：Data Definition Language，数据定义语言，用来维护存储数据的结构（数据库，表），代表指令：create，drop，alter等。

- DML：Data Manipulation Language，数据操作语言，用来对数据进行操作（数据表中的内容），代表指令：insert，delete，update等。其中DML内部又单独进行了一个分类：DQL（Data Query Language:数据查询语言，如select）

- DCL：Data Control Language，数据控制语言，主要是负责权限管理（用户），戴白哦指令：grant，revoke等。



SQL是关系型数据库的操作指令，SQL是一种约束，但不强制（类似W3C）；不同的数据库产品（如Oracle，mysql）可能内部会有一些细微的差别。

### 1.4 Mysql数据库



MySQL数据库是一种C/S结构的软件：客户端/服务端，若想访问服务器必须通过客户端（服务器一直运行，客户端在需要使用的时候运行）



交互方式

1、 客户单连接认证：连接服务器，认证身份：mysql.exe -hPup



mysql.exe -hlocalhost -P3306 -uroot -p

（其中-h找主机 -P找端口软件 -u用户名 -p密码）

![连接成功](https://upload-images.jianshu.io/upload_images/6770220-0a3af67e3e491d14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



2、 客户端发送SQL指令



3、 服务器接收SQL指令：处理SQL指令，返回操作结果

4、 客服端接受结果：显示结果

 show databases; --查看所有数据库

![查看数据库](https://upload-images.jianshu.io/upload_images/6770220-374b09f68d92a7f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



5、 断开连接（释放资源：服务器开发限制）

三种方式退出：exit / quit / \q

![退出数据库](https://upload-images.jianshu.io/upload_images/6770220-4dec05328f5cb75b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**其中的错误**

1、cmd无法打开MySQL

解决方法：[MySQL怎么配置环境变量](https://jingyan.baidu.com/article/b7001fe12931404e7282dddb.html)

2、启动MySQL报错:ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost' (10061)
![未开启MySQL服务报错](https://upload-images.jianshu.io/upload_images/6770220-e8be247d55d83dc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


解决方法：[启动MySQL报错:ERROR 2003 (HY000): Can't connect to MySQL server on 'localhost' (10061)](https://blog.csdn.net/BigData_Mining/article/details/88344513)

### 1.5 MySql服务器对象



没有办法完全了解服务器内部的内容，只能粗略的去分析数据库服务器的内部的结构。



将mysql服务器内部对象分成了四层：系统（DBMS）->数据库（DB）-> 数据表（Table） -> 字段（field)

![MySQL内部分层](https://upload-images.jianshu.io/upload_images/6770220-1c3758c2bf942f88.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 二、SQL基本操作

基本操作：CRUD

将SQL的基本操作根据操作对象进行分类（分三类）：库操作、表操作（字段）、数据操作

### 2.1 库操作

对数据库的增删改查

#### 2.2 新增数据库

**基本语法**

	Create database 数据库名字 [库选项]

![创建数据库](https://upload-images.jianshu.io/upload_images/6770220-0fb0fd7337baabcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


库选项：用来约束数据库，分为两个选项

 - 字符集设定：charset/character 具体字符集（数据存储的编码格式）：常用字符集：GBK和UTF8

 - 校对集设定：collate 具体校对集（数据比较的规则）

注：-- 双中划线+空格：注释（单行注释），也可以使用# 号



注意：数据库名字不能用关键字（已经被使用的字符）或者保留字（将来可能会用到的）

![使用关键字创建数据库报错](https://upload-images.jianshu.io/upload_images/6770220-471296ac44785176.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


如果非要使用关键字或者保留字，那么必须使用反引号（esc键下面的键在英文状态下的输出··）

例如：
	create database \`database` charset utf8;

![反引号](https://upload-images.jianshu.io/upload_images/6770220-7979f967c1ad6311.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



中文数据库是可以的，但是要有前提条件：保证服务器能够识别（别的不用）-->  解决方案：告诉服务器当前中文的字符集是什么：

	set names gbk;
	create database 中国 charset utf8;


![创建中文数据库](https://upload-images.jianshu.io/upload_images/6770220-6e385c5c4390fcc7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**当创建数据库的SQL语句执行后，发送了什么？**


1、在数据库系统中，增加了相应的数据库信息

2、会在保留数据的文件夹下：Data 目录，创建一个对应数据库名字的文件夹

![创建的三个数据库](https://upload-images.jianshu.io/upload_images/6770220-6c30cee1a8b2e5c4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


3、每个数据库下面都有一个opt文件，保存了库选项


![opt文件](https://upload-images.jianshu.io/upload_images/6770220-76d4e3fa702b8fda.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#### 1.2.1 查看数据库



1、查看所有数据库：show databases;

![查看所有数据库](https://upload-images.jianshu.io/upload_images/6770220-e65b2ba3dc11c630.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



2、查看指定部分的数据库：模糊查询

Show databases like 'pattern';  --pattern是匹配模式



%:表示匹配多个字符

_:表示匹配单个字符



例如:

-- 创建数据库

create database information charset utf8;


-- 查看以information_开始的数据库： _需要被转义

show databases like 'information\_%';

show databases like 'information_%'; --相当于information%


![注意_的转义](https://upload-images.jianshu.io/upload_images/6770220-7801c50dbd209059.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3、查看数据库的创建语句：

show create database 数据库名字；

show create database `database`； --关键字命名的数据库要反引号

![查看数据库的创造语句](https://upload-images.jianshu.io/upload_images/6770220-6821e16a4b2c684f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 1.2.2 更新数据库

数据库名字不可以修改。

（很低版本可以修改，但高版本不可以修改，因为可修改将导致不安全）


数据库的修改仅限库选项：字符集和校对集（校对集以来字符集）

Alter database 数据库名字 [库选项]

库选项：

Charset/character set [-] 字符集

Collate 校对集


例如：alter database informationtest charset gbk;

修改前：
![修改前](https://upload-images.jianshu.io/upload_images/6770220-80a7bd0710a9b5c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

修改后：
![修改后](https://upload-images.jianshu.io/upload_images/6770220-f95b0c6a7e83cf45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1.2.3 删除数据库 



所有的操作中，删除是最简单的。



Drop database 数据库名；



当删除数据库语句执行后，发送了什么？

1、在数据库内部看不到相应的数据库

2、在对应的数据库存储的文件夹内，数据库名字对应的文件夹也被删除（级联删除：里面的数据表全部删除）

**注意：数据库的删除不是闹着玩的，不要随意删除，应该先进行备份后操作（删除不可逆）**



### 2.2 表操作



表和字段是密不可分的



#### 2.2.1 新增数据表



Create table [if not exists] 表面名（

字段名字 数据类型，

字段名字 数据类型  -- 最后一行不需要逗号

）[表选项]；



if not exists: 如果表明不存在，那么就创建，否则不执行创建代码；检查功能



表选项：控制表的表现

  字符集：charset/character set 具体字符集； -- 保证表中数据存储的字符集



  校对集：collate 具体校对集；



  存储引擎：engine具体的存储引擎（innodh和myisam）



-- 创建表

	
	
	create table if not exists student(
	name varchar(10),
	gender varchar(10),
	number varchar(10),
	age int
	)charset utf8;



运行报错，原因：任何一个表的设计都必须指定数据库，

![任何一个表的设计都必须指定数据库](https://upload-images.jianshu.io/upload_images/6770220-41f0b9fa75f72c6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


解决方案
 
方案一：显示指定表所属的数据库

Create table 数据库名。表名（）； -- 将当前数据表创建到指定的数据库下


![方式一](https://upload-images.jianshu.io/upload_images/6770220-dc06c51bf48146c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


方案二：隐式的制定表所属的数据库：先进入到某个数据库环境，然后这样创建的表自动归属到某个指定的数据库。



进入数据库环境：use 数据库名字；

![方式二](https://upload-images.jianshu.io/upload_images/6770220-c2951953bf870ce7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


当创建数据表的SQL指令执行之后，到底发送了什么？

1、指定数据库下已经存在相应的表

2、在数据库对应的文件夹下，会产生对应表的结构文件（跟存储引擎有关系）

![产生结构文件](https://upload-images.jianshu.io/upload_images/6770220-c0fb39dfadea7868.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 2.2.2 查看数据库



数据库能查看的方式表都能查看，并多出一个查看表结构的功能。



1、查看所有表：show tables;

![查看表](https://upload-images.jianshu.io/upload_images/6770220-d47d76e0b368548d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、查看部分表：模糊匹配：show tables like 'pattern';

例如：查看以s结尾的表

show tables like '%s';(不推荐效率低)


![模糊查找](https://upload-images.jianshu.io/upload_images/6770220-cbaccbad21a6deaf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


3、查看表创建语句：show create table 表名;

show create table student;

show create table student\g   -- \g = ;

show create table student\G -- \G = 将查到的结构90度旋转变成纵向

![查看表创建语句](https://upload-images.jianshu.io/upload_images/6770220-904808b23aad7d8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


4、查看表结构：查看表中字段信息

Desc/describe/show colomns from 表名；

  ![查看表结构](https://upload-images.jianshu.io/upload_images/6770220-92f422b7f347831f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.3 修改数据表



表本身存在，还包含字段：表的修改分为两个部分：表修改本身和修改字段。



#### 修改表本身


表本身可以修改：表名和表选项

- 修改表明：rename table 老表名 to 新表名；

- 修改表选项：字符集，校对集和存储引擎

 alter table 表名 表选项 [=] 值；



- 修改字段

字段操作很多：新增，修改，重命名，删除


![修改字符集](https://upload-images.jianshu.io/upload_images/6770220-eb4cc1f49ab9ec04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


新增字段

Altertable 表名 add[column] 字段名 数据类型 [列属性][位置];

位置：字段名可以存放表中的任意位置

First：第一个位置

After：在哪个字段之后：after 字段名；默认是在最后一个字段后



例如：给学生表增加ID放到第一个位置

alter table my_student

add column id int

first; -- mysql会自动寻找分号作为语句结束符

![新增字段](https://upload-images.jianshu.io/upload_images/6770220-5c91d2b02d78d1e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



修改字段：修改通常是修改属性或者数据类型

Alter table 表名 modify 字段名 数据类型 [属性][位置];



 -- 将学生表中的number学号字段变成固定长度，且d放到id后面
 alter table my_student modify number char(10) after id;


![修改字段属性](https://upload-images.jianshu.io/upload_images/6770220-2ea91abe7be92a95.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



重命名字段

Alter table 表名 change 旧字段 新字段名 数据类型 [属性][位置];



-- 修改学生表中的gender字段为sex

Alter table my_student change gender sex varchar(10);


![修改字段名](https://upload-images.jianshu.io/upload_images/6770220-a9eaac83f1bdda42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


删除字段

Alter table 表名 drop 字段名；

例如：alter table my_student drop age;

小心：如果表中已经存在数据，那么删除字段会清空该字段的所以数据（不可逆）

![删除字段](https://upload-images.jianshu.io/upload_images/6770220-4caa2cec3bc62f57.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.4 删除数据表



Drop table 表名 1，表名 2... ;   -- 可以一次性删除多张表



例如：drop table class;


![删除表](https://upload-images.jianshu.io/upload_images/6770220-27c558ac65beb8e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当删除数据表的指令执行之后发送了什么？

1、在表空间中，没有了指定的表（数据也没有了）。

2、在数据库对应的文件夹下，表对应的文件（与存储引擎有关）也会被删除。

注意：删除有危险，操作需谨慎（不可逆）。

## 三、数据操作

### 3.1 新增数据

有两种方案

方案1：给全表字段插入数据，不需要指定字段列表，但要求数据的值出现的顺序必须与表中设计的字段出现的顺序一致，且凡是非数值数据，都需要使用引号（建议是单引号）包裹。



Insert into 表名 values（值列表）[，（值列表）]；  -- 可以一次性插入多条记录



-- 插入数据

insert into my_student values(1,'001','Jim','male'),(2,'002','Amy','female');


![方式1](https://upload-images.jianshu.io/upload_images/6770220-26aa9333c0bf21d1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


方案2：给部分字段插入数据，需要选定字段列表：字段列表出现的顺序与字段的顺序无关；但是值列表的顺序必须与选定的字段的顺序一致。

Insert into 表名 （字段列表） values （值列表）[，（值列表）];

![方式2](https://upload-images.jianshu.io/upload_images/6770220-c9bec87b657fd7cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



-- 插入数据：指定字段列表

insert into my_student(number,sex,name,id) values 

('003','male','Tom',3),

('004','female','Lily',4);


### 3.2 查看数据

Select */字段列表 from 表名 [where 条件];


-- 查看所有数据

select * from my_student;

![查看所有数据](https://upload-images.jianshu.io/upload_images/6770220-087b75f7f429ce0c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


-- 查看指定字段，指定条件数据

select id,number,sex,name from my_student  where id =1; -- 查看满足id=1 的学生信息

![查看id为1的学生信息](https://upload-images.jianshu.io/upload_images/6770220-d12da7cba46ab5e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3.3 更新数据



Update 表名 set 字段 = 值 [where 条件]；  --建议都有where，如果没有就是更新全部。



-- 跟新jim的性别

update my_student set sex = 'female' where name = 'Jim';

![更新数据](https://upload-images.jianshu.io/upload_images/6770220-6ae4fceaa8bda8b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

更新不一定会成功，如没有真正需要更新的数据。

![并不是指令运行就是成功](https://upload-images.jianshu.io/upload_images/6770220-d074f009c0b884ef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.4 删除数据



删除是不可逆的：谨慎删除



-- 删除数据

delete from my_student where sex = 'male';

![删除数据](https://upload-images.jianshu.io/upload_images/6770220-f99573fb46f87b70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 四、中文数据问题



中文数据问题本质是字符集问题。



计算机只识别二进制，人类更多是识别符号，需要有个二进制与字符的对应关系（字符集）



-- 插入数据（中文）

insert into my_student values(5,'005','张越','男');


客户端向服务器插入中文数据，没有成功。



原因：汉字在当前编码（字符集）下对应的二进制编码换成的十六进制：两个汉字=》四字节（GBK）



报错：服务器没有识别对应的四个字节：服务器认为数据是UTF8，一个汉字有三个字节，读取三个字节转换成汉字（失败），剩余的再读三个字节（不够），最终失败。



所有的数据库服务器表现的一些特性都是通过服务器的变量来保存：系统先读取自己的变量，看看应该怎么表现。



//查看服务器到底识别哪些字符集

show character set;


![可识别的字符集](https://upload-images.jianshu.io/upload_images/6770220-beb669ab6b98e932.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


基本上：服务器是万能，什么字符集都能支持。

 

//既然服务器识别那么多，总有一种是服务器默认的跟客户端打交道的字符集。



-- 查看服务器默认的对外处理的字符集

show variables like 'character_set%';

![默认对外字符集](https://upload-images.jianshu.io/upload_images/6770220-066a7f441dcc6d49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


问题根源：客户端数据只能是GBK，而服务器认为是UTF，矛盾产生。



解决方案：改变服务器，默认的接收字符集为GBK；

Set character_set_client = gbk;



-- 修改服务器认为的客户端数据的字符集为GBK

Set character_set_client = gbk;



![添加成功](https://upload-images.jianshu.io/upload_images/6770220-d2bbad1c9216699b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果显示表数据，发现中文乱码，这样的原因是：数据来源是服务器，解析数据是客户端（客户端只识别GBK:智慧两个字节一个汉字），但是事实服务器给的数据确实UTF8，三个字节ig汉字->乱码



解决方式:修改服务器给定数据的字符集为GBK

set character_set_result = gbk;



![成功显示](https://upload-images.jianshu.io/upload_images/6770220-51907d17b208383a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




Set 变量 = 值；修改只是会话级别（当前客户端，当西连接有效，关闭失效）



设置服务器对客户端的字符集的认识：可以使用快捷方式：set names 字符集

Set names gbk; ==> character_set_client,character_set_result,character_set_connection



Connection连接层：是字符集转变的中间者，如果统一了效率更高，不同意也没问题。

## 五、校对集问题



校对集：数据比较的方式



校对集有三种格式

- _bin:binary,二进制比较 ，去除二进制位，一位一位的比较，区分大小写

- _cd:case sensitive,大小写敏感，区分大小写

- _ci:case insensitice,大小写不敏感，不区分大小写



查看所有校对集

show collation;

默认校对集：
![默认校对集](https://upload-images.jianshu.io/upload_images/6770220-938a22283676a159.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


校对集应用：只有当数据产生比较的时候，校对集才会生效。



对比：使用UTF8的_bin和_ci的不同校对集;



1、-- 创建表使用不同的校对集

create table my_collate_bin( name char(1))charset utf8 collate utf8_bin;



create table my_collate_ci(name char(1))charset utf8 collate utf8_general_ci;



2、-- 插入数据

insert into my_collate_bin values('a'),('A'),('B'),('b');



insert into my_collate_ci values('a'),('A'),('B'),('b');



-- 查看 

select * from my_collate_bin;



select * from my_collate_ci;

![查看表](https://upload-images.jianshu.io/upload_images/6770220-5e25275b16be8ea0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	

3、-- 比较：根据某个字段进行排序：order by 字段名 [asc|desc]; asc升序，dese降序，默认升序

-- 排序查找

select * from my_collate_bin order by name;

select * from my_collate_ci order by name;


 
![比较后](https://upload-images.jianshu.io/upload_images/6770220-bcdfee6590ad5f68.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


校对集:必须在没有数据之前声明号，如果有了数据，那么再进行校对集修改，那么修改无效。



-- 有数据后修改校对集无效

alter table my_collate_ci collate = utf8_bin;

![后期修改无效](https://upload-images.jianshu.io/upload_images/6770220-9ef96fd475f8038e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 六、Web乱码问题



动态网站由三部分构成：浏览器，apache服务器（PHP），数据库服务器，三个部分都有自己的字符集（中文），数据需要在三个部分之间来回传递，很容易产生乱码。



如何解决乱码问题：统一编码（三码合一）



但事实上不可能：浏览器是用户管理（根本不可能控制）



但是必须要解决这些问题：主要靠PHP来做

![乱码问题解决](https://upload-images.jianshu.io/upload_images/6770220-a184477f5990692c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
