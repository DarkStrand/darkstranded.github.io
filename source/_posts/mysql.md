---
title: mysql
author: DarkStrand
avatar: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/avatar.jpg
authorLink: DarkStrand.cn
authorAbout: 一个神奇的小伙
authorDesc: 一个神奇的小伙
categories: 技术
date: 2021-08-13 08：38：25
comments: true
tags: 
 - web
keywords: mysql
description: mysql
photos: https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/4.jpg

---



# 1. 安装

[原文](https://blog.csdn.net/qq_41134710/article/details/116406099)



## 1.1 安装mysql

安装mysql的方法主要有“通过命令行”和“可视化界面安装”这两种方法。这里是“可视化界面安装”的方法。



1. 登录Mysql官网

[mysql下载的官网](https://www.mysql.com/downloads/)

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/3.png)



2. 一般我们下载“社区办的mysql”，所以需要点击下图的 Mysql Community(GPL)Downloads。

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/4.png)



3. 然后选择 Mysql Community Server

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/5.png)



4. 接下来进入现在页面，这里有这个下载的链接，我们选择“DMG格式”的下载链接。注意：一定要选择macOS系统。

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/6.png)



5. 下载了dmg格式的安装包之后，接下来的安装就比较简单了，需要注意的是：

   1. 一定要选择 Use Legacy Password Encryption。

      ![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/7.png)

   2. 一定要记得输入密码，这个密码也是登录mysql的密码，非常重要。备注：如果是8.23版本之后的Mysql，那么在输入密码的时候需要至少输入8位。

      ![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/8.png)

6. 剩下的安装只需要点击继续就可以了。

7. 查看Mysql是否安装成功，需要点击“MAC中的系统偏好设置”，然后点击Mysql图标

   ![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/9.bmp)

8. 进入 Mysql 界面之后，如果是两个绿色的原型图标，则说明 Mysql已经安装完成了。

   ![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/10.bmp)

   

## 1.2 配置Mysql

如果需要在终端里面输入Mysql命令，那么就需要进行如下的配置。

1. 打开文件: `open ~/.bash_profile`
2. 加入语句`PATH=$PATH:/usr/local/mysql/bin`
3. 使配置的语句生效：`source ~/.bash_profile`

配置成功后的语句如下：

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/11.png)



如图所示只需要加入最后一行即可。

如果配置成功，那么输入命令： `mysql -uroot -p`，运行效果如下：

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/12.png)

此时输入登录密码就可以使用了。

如果不能出现上面的运行图片，则说明配置环境失败。



## 1.3 配置Mysql环境变量遇到的坑

之前我们是在 bash 环境中配置 Mysql，所以当电脑重启或者关机之后有可能出现 mysql 命令失效的情况。这也是一个坑，解决办法如下：

```
# 在~/.zshrc文件最后，增加一行：
source ~/.bash_profile
```

1. 如果没有 `~/.zshrc` 文件，那么就需要执行 `touch ~/.zshrc`；反之，如果有 zsh 文件，那么可以跳过本步骤，直接进入第2步。

2. 执行 `open ~/.zshrc` 命令，然后添加 `source ~/.bash_profile` 即可；如下图所示；

   ![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/13.png)

3. 然后就可以解决电脑重启后mysql命令失效的问题了。










# 2. 卸载

> 卸载MySQL，首先得知道MySQL的路径。默认的话是在`/usr/local`文件夹下的。
>  在`系统偏好设置`面板中可以看到之前安装的MySQL，此时若想卸载MySQL，可以按照如下步骤来。【之前安装的时候采用的是默认路径的安装，所以符合下面的卸载步骤】



![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/1.webp)



```
# 步骤一：切换到~
cd ~
# 步骤二：打开usr文件
open /usr
# 步骤三：找到local，进入到local文件夹，然后依次执行如下命令
sudo rm /usr/local/mysql
sudo rm -rf /usr/local/mysql*
sudo rm -rf /Library/StartupItems/MySQLCOM
sudo rm -rf /Library/PreferencePanes/My*
rm -rf ~/Library/PreferencePanes/My*
sudo rm -rf /Library/Receipts/mysql*
sudo rm -rf /Library/Receipts/MySQL*
sudo rm -rf /var/db/receipts/com.mysql.*
# 步骤四：卸载完成，可以发现在`系统偏好设置`没有了mysql的标志
```

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/2.bmp)



**PS:**

> 如果你不是默认路径安装的或者忘记了是不是默认路径安装的，那么除了执行上面的命令之外，还要检查以下文件中是否有对应的文件，有的话删除即可。



* 检查/usr/local/Cellar目录是否有mysql文件，有的话删除。

* 检查/usr/local/var 里的mysql文件，有的话删除。

* 检查/tmp 里的mysql.sock、mysql.sock.lock、 my.cnf文件，有的话删除。

* err文件以及pid文件都是在/usr/local/var/mysql中，有的话删除。

* brew安装的安装包存储在/usr/local/Library/Cache/Homebrew，有的话删除。

* 一定要记得执行brew cleanup。





# 3. 语法

[原文](https://link.juejin.cn/?target=https%3A%2F%2Fblog.csdn.net%2Fjavaee_gao%2Farticle%2Fdetails%2F106633872)

SQL语句广义可以分为以下三种类型：

1. DDL（Data Definition Language）：数据定义语言，这种类型的语句定义了对数据库对象（database、index、table、column）的操作，常用的SQL关键字有`create`、`alter`、`drop`等。
2. DML（Data Manipulation Language）：数据操纵语言，这种类型的语句用于对数据库存储数据列进行增删改查操作，常见的SQL关键字有`insert`、`delete`、`update`、`select`等。
3. DCL（Data Control Language）：数据控制语言，这种类型的数据定义了对数据库表、字段、用户访问许可与安全级别的操作，常见的关键字有`grant`、`revoke`等。



## 3.1 DDL

下面简单通过一些案例演示DDL语句的用法



### 3.1.1 连接Mysql数据库

需启动Mysql相关服务后，如果你的Mysql实例加入了系统服务可以键入如下命令以查看mysql服务状态。

```
systemctl status 你的mysql系统服务名
```

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/14.png)



如果你检查mysql服务没有启动如下图所示：

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/15.png)

这个时候执行一下启动mysql服务命令即可。

```
systemctl start mysqld
```





> m1mac连接mysql

打开终端，为Path路径附加MySQL的bin目录

```
PATH="$PATH":/usr/local/mysql/bin
```

然后通过以下命令登录MySQL

```
mysql -u root -p
```

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/16.png)



登录成功，但是运行命令的时候会报错，提示我们需要重设密码。

```
mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.
mysql>
```



修改密码，新密码为root

```
set PASSWORD =PASSWORD('root');
```



> 忘记密码

点击系统偏好设置->最下边点MySQL，在弹出页面中，关闭服务

![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/17.png)



进入终端输入

```
cd /usr/local/mysql/bin/
```

回车后登录管理员权限

```
sudo su
```

回车后输入以下命令来禁止mysql验证功能

```
./mysqld_safe --skip-grant-tables &
```

回车后mysql会自动重启（偏好设置中mysql的状态会变成running）



输入命令

```
./mysql -p
```

回车后，输入命令

```
FLUSH PRIVILEGES
```

回车后，输入命令修改密码

```
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('你的新密码');
```

或者使用下面这个更改密码

```
update mysql.user set authentication_string=password('root') where user='root' and Host ='localhost';
```





### 3.1.2 创建数据库

当确定mysql服务正常启动后，这个时候就可以创建数据库了。输入以下命令即可连接数据库。

```
 mysql -h 127.0.0.1 -P 3306 -u root -p
 
 # 或者输入
 mysql -u root -p
```



![](https://cdn.jsdelivr.net/gh/Darkstranded/CDN/images/mysql/18.png)

在上面命令行中使用了几个重要的参数，且欢迎界面有一些描述信息。

* mysql 代表着客户端连接命令
* -h 表示指定mysql客户端连接mysql服务端的ip地址
* -P 表示连接指定mysql服务器运行的端口号
* -u 表示以指定的用户名去连接mysql服务器
* -p 表示连接mysql服务器需要输入密码
* mysql命令的结束符用`;`或者`\g`结束

* 每一个连接mysql都会为这次连接分配一个id，这个id表示mysql服务的连接次数
* 这里我使用的是mysql 8.0.27 社区版本
* 通过输入`help`，或者`\h`获取帮助内容，输入`\c`清除当前输入的内容



接下来让我们创建一个数据库，其语法如下所示：

```
CREATE DATABASE databaseName;
```

例如我们创建一个databaseName为test的数据库如下所示：

```
mysql> create database test;
Query OK, 1 row affected (0.00 sec)
mysql>
```

如果你看到上面有`Query OK`选项，恭喜你已经成功执行了创建数据库命令。但如果创建已经存在的database对象则会报错。例如我们再次创建database为test的数据库对象，系统将会提示：

```
mysql> create database test;
ERROR 1007 (HY000): Can't create database 'test'; database exists
mysql>
```



可以查询当前数据实例拥有哪些数据库名称以避免上述的错误，我们输入如下命令：

```
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
5 rows in set (0.00 sec)
mysql>
```



当查看当前所拥有的数据库对象后，需要继续切换到需要操作的数据库对象。切换数据库语法为：

```
USE databaseName;
```

例如我们切换到我们之前创建的数据对象`test`：

```
mysql> use test;
Database changed
mysql>
```

当切换到具体的数据库对象，我们还可以使用命令查询当前数据库对象下保存所有表对象：

```
mysql> show tables;
Empty set (0.00 sec)
mysql>
```

如果数据库对象下没有表对象则会提示`Empty set`



### 3.1.3 删除数据库

如果需要删除数据库对象，其语法为：

```
DROP DATABASE 数据库名;
```

例如删除我们创建的test数据库对象：

```
mysql> drop database test;
Query OK, 0 rows affected (0.01 sec)
mysql>
```

> 注意⚠️！！！ 删除数据库对象后，所属的数据库对象下的表也将会被全部删除，所以请谨慎执行删除数据库对象的命令。



### 3.1.4 创建表

在数据库中创建表对象的语法如下：

```
CREATE TABLE tablename 
(column_name1, column_value1 约束条件，
column_name2, column_value2 约束条件，
column_name3, column_value3 约束条件， 
)
```

例如创建一个tb_person对象表，这个对象拥有姓名（name）、年龄（age）、性别（genderg）三个属性，其创建命令如下所示：

```
mysql> create table tb_person(name varchar(50) not null, age int(11) not null, gender varchar(10) not null);
Query OK, 0 rows affected (0.01 sec)
mysql>
```



当创建表后可以键入如下命令查看表详情信息语法如下：

```
DESC 表名；
```

例如查看tb_person表：

```
mysql> desc tb_person;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| name   | varchar(50) | NO   |     | NULL    |       |
| age    | int(11)     | NO   |     | NULL    |       |
| gender | varchar(10) | NO   |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
mysql>
```



如果想要看当前表使用的sql引擎、表字符集相关定义、以及创建的表语句可以使用如下命令：

```
mysql> show create table tb_person;
+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table     | Create Table                                                                                                                                             |
+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
| tb_person | CREATE TABLE `tb_person` (
  `name` varchar(50) NOT NULL,
  `age` int(11) NOT NULL,
  `gender` varchar(10) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+-----------+----------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```



### 3.1.5 删除表

删除表的语法与删除数据库对象类似，其语法如下：

```
DROP TABLE 表名
```

例如删除表tb_person：

```
mysql> drop table tb_person;
Query OK, 0 rows affected (0.01 sec)
mysql>
```



### 3.1.6 修改表

对于已经建立好的表，如果需要对表结构做修改如增加列、删除列或者修改列属性。可以将表drop掉重新建立新表，但是这对那些已经表中拥有大量数据的表并不合适。尝尝通过是使用`alter table`语句其常见修改情形如下所示：



* 修改表列的属性

一般有时候需要将列的类型进行更改，其修改语法如下所示：

```
ALTER TABLE TABLENAME MODIFY COLUMN 列名A 列属性 [FIRST | AFTER 列名B]
```

例如我们将tb_person中的age列属性由`int`更改为`varchar`类型：

```
mysql> alter table tb_person modify column age varchar(20);
Query OK, 1 row affected (0.02 sec)
Records: 1  Duplicates: 0  Warnings: 0
mysql>
```

此时我们查看一下tb_person的表定义信息如下：

```
mysql> desc tb_person;
+--------+-------------+------+-----+---------+-------+
| Field  | Type        | Null | Key | Default | Extra |
+--------+-------------+------+-----+---------+-------+
| name   | varchar(50) | NO   |     | NULL    |       |
| age    | varchar(20) | YES  |     | NULL    |       |
| gender | varchar(10) | NO   |     | NULL    |       |
+--------+-------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
```

可以看到age属性由之前的int类型更改为varchar类型了。



* 增加表列信息

如果需要对已经定义好的表新增一列，其修改语法如下所示：

```
ALTER TABLE TABLENAME ADD COLUMN 列名A 列属性 约束条件 [AFTER | FIRST 列名B]
```

例如我们在表中age后新增一个salary的列属性：

```
mysql> alter table tb_person add column salary decimal(10,2) not null after age;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
mysql>
```

此时我们查看一下tb_person的表定义信息如下：

```
mysql> desc tb_person;
+--------+---------------+------+-----+---------+-------+
| Field  | Type          | Null | Key | Default | Extra |
+--------+---------------+------+-----+---------+-------+
| name   | varchar(50)   | NO   |     | NULL    |       |
| age    | varchar(20)   | YES  |     | NULL    |       |
| salary | decimal(10,2) | NO   |     | NULL    |       |
| gender | varchar(10)   | NO   |     | NULL    |       |
+--------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
mysql>
```

此时可以看到在age列下新增了salary字段。



* 删除表字段

删除表字段的语法如下所示：

```
ALTER TABLE TABLENAME DROP COLUMN 列名
```

例如我们删除掉gender字段：

```
mysql> alter table tb_person drop column gender;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0
mysql>

mysql> desc tb_person;
+--------+---------------+------+-----+---------+-------+
| Field  | Type          | Null | Key | Default | Extra |
+--------+---------------+------+-----+---------+-------+
| name   | varchar(50)   | NO   |     | NULL    |       |
| age    | varchar(20)   | YES  |     | NULL    |       |
| salary | decimal(10,2) | NO   |     | NULL    |       |
+--------+---------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
mysql>
```



* 修改列名

有时候新建立的表名难免有欠缺的地方其修改语法如下所示：

```
ALTER TABLE TABLENAME CHANGE COLUMN 旧列名 新列名 列属性 约束 
```

例如将age列名修改为address如下所示：

```
mysql> alter table tb_person change column age address varchar(50) not null;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
mysql>

mysql> desc tb_person;
+---------+---------------+------+-----+---------+-------+
| Field   | Type          | Null | Key | Default | Extra |
+---------+---------------+------+-----+---------+-------+
| name    | varchar(50)   | NO   |     | NULL    |       |
| address | varchar(50)   | NO   |     | NULL    |       |
| salary  | decimal(10,2) | NO   |     | NULL    |       |
+---------+---------------+------+-----+---------+-------+
3 rows in set (0.00 sec)
mysql>
```

> 或你注意到modify与change都可以更改列属性，但不同的是change可以修改列名，modify无法修改列名。



* 更改表名

更改表名的语法如下所示：

```
ALTER TABLE TABLENAME RENAME TO 新表名
```

例如将表名tb_person修改为tb_per如下所示：

```
mysql> alter table tb_person rename to tb_per;
Query OK, 0 rows affected (0.00 sec)
mysql>

mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| tb_per         |
+----------------+
1 row in set (0.00 sec)
mysql>
```





## 3.2 DML

DML语句是我们平时开发中常常打交道的SQL语句，其主要包括CRUD操作即所谓的增删改查。



### 3.2.1 表中插入数据

插入表记录的语法如下所示：

```
INSERT INTO TABLENAME(列名A,列名B,列名C) values (列值1，列值2，列值3) 
```

例如我们向表`tb_person`中添加一条数据如下所示：

```
mysql> insert  into tb_person(name,address,salary) values('张三','上海','15000');
Query OK, 1 row affected (0.00 sec)
```



如果你插入的数据报 **Incorrent String value xxx** 的异常，这通常是数据库对象编码集的问题，所以我们设置一下编码集，关于数据库中文字符集的问题可以参考 [解决Mysql中文插入异常处理](https://blog.csdn.net/javaee_gao/article/details/102383697)。



如果一个表中的所有列都要进行值插入我们还可以简写为如下：

```
mysql> insert  into tb_person values('张三','上海','15000');
Query OK, 1 row affected (0.00 sec)
mysql>
```

Mysql还支持批量插入数据，其语法如下所示：

```
INSERT INTO TABLENAME(列名A,列名B,列名C) values (列值1，列值2，列值3),
values (列值4，列值5，列值6),values (列值7，列值8，列值9) 
```

例如我们在表中添加多条记录如下所示：

```
mysql> insert  into tb_person(name,address,salary) values('张三','上海','15000'),('李四','北京','18000'),('王五','西安','9500');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0
mysql>
```

一般在大量的数据插入的时候特别有效。



### 3.2.2 更新表记录

当需要对表中的数据进行更新时，其语法如下所示：

```
UPDATE TABLENAME SET 列1=值1,列2=值2,列3=值3 where 条件表达式 and 条件表达式...
```

例如我们将表中姓名为张三，地址为23的修改为赵六、南京，薪水10000如下：

```
mysql> update tb_person set name='赵六', address ='南京', salary ='10000' where name='张三' and address ='23';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```



### 3.2.3 删除记录

如果记录不需要可以进行删除，则可用delete命令语法如下：

```
DELETE FROM 表名 WHERE 条件表达式 and 条件表达式 ....
```

```
mysql> select * from tb_person;
+--------+---------+----------+
| name   | address | salary   |
+--------+---------+----------+
| 赵六   | 南京    | 10000.00 |
| 张三   | 上海    | 15000.00 |
| 张三   | 上海    | 15000.00 |
| 张三   | 上海    | 15000.00 |
| 李四   | 北京    | 18000.00 |
| 王五   | 西安    |  9500.00 |
+--------+---------+----------+
6 rows in set (0.00 sec)
```

例如，在 tb_person 中删除 name=张三,addres=上海的表记录。

```
mysql> delete from tb_person where name='张三' and address='上海';
Query OK, 3 rows affected (0.00 sec)
```



### 3.2.4 查询记录

数据插入数据库中，就可以用 SELECT 命令进行各行各样的查询，其查询语法如下：

```
select 列名1,列名2,列名3 from TABLENAME where 条件表达式 and 条件表达式 ....
```

例如查询 tb_person 表中所有记录如下所示：

```
mysql> select * from tb_person;
+--------+---------+----------+
| name   | address | salary   |
+--------+---------+----------+
| 赵六   | 南京    | 10000.00 |
| 李四   | 北京    | 18000.00 |
| 王五   | 西安    |  9500.00 |
+--------+---------+----------+
3 rows in set (0.00 sec)
```

其中 * 表示匹配表中的所有列，其等价于下列查询语句：

```
select name,address,salary from tb_person;
```



* 查询不重复记录

我们现在向这张表中插入重复数据如下所示：

```
mysql> insert into tb_person values('川建国','America',100000),('蓬佩奥','America',10000),('川建国','America',100000);
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0
```

重新查询所有列数据如下所示：

```
mysql> select name,address,salary from tb_person;
+-----------+---------+-----------+
| name      | address | salary    |
+-----------+---------+-----------+
| 赵六      | 南京    |  10000.00 |
| 李四      | 北京    |  18000.00 |
| 王五      | 西安    |   9500.00 |
| 川建国    | America | 100000.00 |
| 蓬佩奥    | America |   10000.00 |
| 川建国    | America | 100000.00 |
+-----------+---------+-----------+
6 rows in set (0.00 sec)
```



我们可以看到表中查询出来有三处重复的数据，此时可以使用`distinct`过滤重复列如下所示：

```
mysql> select distinct name,address,salary from tb_person;
+-----------+---------+-----------+
| name      | address | salary    |
+-----------+---------+-----------+
| 赵六      | 南京    |  10000.00 |
| 李四      | 北京    |  18000.00 |
| 王五      | 西安    |   9500.00 |
| 川建国    | America | 100000.00 |
| 蓬佩奥    | America |   10000.00 |
+-----------+---------+-----------+
5 rows in set (0.00 sec)
```



* 条件查询

很多情况下条件查询是我们经常打交道的SQL查询，可以使用`Where`搭配多个条件进行查询，例如查询`address=America`的全部记录如下所示：

```
mysql> select * from tb_person where address = 'America';
+-----------+---------+-----------+
| name      | address | salary    |
+-----------+---------+-----------+
| 川建国    | America | 100000.00 |
| 蓬佩奥    | America |  10000.00 |
| 川建国    | America | 100000.00 |
+-----------+---------+-----------+
3 rows in set (0.00 sec)
```

在条件查询中组合条件可以使用`and`表示`与`关系，`or`表示`或`关系。如果是数值类型的还可以使用数字比较运算符如`>`、`<`、`=`、`!=`、`<=`、`>=`。例如查询`address=America & salary>=20000`的表记录如下所示：

```
mysql> select * from tb_person where address='America' and salary>='20000';
+-----------+---------+-----------+
| name      | address | salary    |
+-----------+---------+-----------+
| 川建国    | America | 100000.00 |
| 川建国    | America | 100000.00 |
+-----------+---------+-----------+
2 rows in set (0.00 sec)
```



* 列排序以及限制

一般在开发中也有着对某一列进行排序并限制结果集语法如下所示：

```
SELECT 列名1,列名2 .... from TABLENAME where 条件表达式 and|or 条件表达式.... ORDER BY 列名1，列名2 DESC|ASC
```

例如我们对表中列按照薪水大小倒排序并输出3条记录如下所示：

```
mysql> select * from tb_person order by salary desc limit 3;
+-----------+---------+-----------+
| name      | address | salary    |
+-----------+---------+-----------+
| 川建国    | America | 100000.00 |
| 川建国    | America | 100000.00 |
| 李四      | 北京    |  18000.00 |
+-----------+---------+-----------+
3 rows in set (0.01 sec)
```

上面`DESC`与`ASC`是排序顺序的关键字，前者表示`倒叙`排序后者表示`顺序`排序。如果排序的字段值一致，且只有一个排序字段则表中相同的记录将会无序排列，如果有第二个排序字段则按照第二个排序字段进行排序，以此类推。

```
mysql> select * from tb_person order by salary desc,address asc ;
+-----------+---------+-----------+
| name      | address | salary    |
+-----------+---------+-----------+
| 川建国    | America | 100000.00 |
| 川建国    | America | 100000.00 |
| 李四      | 北京    |  18000.00 |
| 蓬佩奥    | America |  10000.00 |
| 赵六      | 南京    |  10000.00 |
| 王五      | 西安    |   9500.00 |
+-----------+---------+-----------+
6 rows in set (0.00 sec)
```



其中上面你可以看到我们在一些SQL语句后面加入了`limit`关键字，关于`limit`的语法如下所示：

```
SELECT * from 表名 where 条件表达式1 and 条件表达式2  LIMIT [offset count]
```

其中`offset`指的是表中起始偏移量，`count`表示需要查询的记录数。例如从表中第二行开始查询3条记录如下所示：

```
mysql> select * from tb_person order by salary desc limit 1,3;
+-----------+---------+-----------+
| name      | address | salary    |
+-----------+---------+-----------+
| 川建国    | America | 100000.00 |
| 李四      | 北京    |  18000.00 |
| 赵六      | 南京    |  10000.00 |
+-----------+---------+-----------+
3 rows in set (0.00 sec)
```



* 分组查询

一般情况下会对查询进行分组处理，如按照地区分组查询每个地区的总人数，其语法如下所示：

```
SELECT FUNCTION(列名) FROM 表名 where 条件表达式1 and|or 条件表达式2 GROUP BY 列名 HAVING 条件表达式
```

其中`FUNCTION`一般指的是聚合函数如`SUM`、`COUNT`、`AVG`、`MAX`、`MIN`，`GROUP BY`表示按照列名进行分组。

`HAVING`对查询出来的结果在按照条件进行过滤。

> 这里需要注意 HAVING 与 where 区别，HAVING 是对聚合函数的结果进行过滤。而 WHERE 是对聚合前对表查询的记录进行过滤。

例如以 address 分组统计人数以及薪水之和如下所示：

```
mysql> select address, count(*),sum(salary) from tb_person group by address;
+---------+----------+-------------+
| address | count(*) | sum(salary) |
+---------+----------+-------------+
| America |        3 |   210000.00 |
| 北京    |        1 |    18000.00 |
| 南京    |        1 |    10000.00 |
| 西安    |        1 |     9500.00 |
+---------+----------+-------------+
4 rows in set (0.00 sec)
```

在此基础上查询薪水之和大于10000的记录如下所示：

```
mysql> select address, count(*),sum(salary) from tb_person group by address having sum(salary)>10000;
+---------+----------+-------------+
| address | count(*) | sum(salary) |
+---------+----------+-------------+
| America |        3 |   210000.00 |
| 北京    |        1 |    18000.00 |
+---------+----------+-------------+
2 rows in set (0.00 sec)
```



* 表连接查询

在日常开发中一般业务不会存在一张表上，根据实际的业务需求将业务数据保存在不同的表上，这里就牵扯到多表查询。从连接类型上可分为`内连接`、`外连接`，它们之前的区别就是`内连接`匹配的是两表中都`相互匹配`的数据，而`外连接`会匹配其他`不匹配`的记录。



这里我们先新建部门表并添加几条数据如下所示：

```
mysql> create table tb_department(id int primary key auto_increment,name varchar(30));
Query OK, 0 rows affected (0.01 sec)

mysql> insert into tb_department(name) values('技术部门'),('hr'),('产品线');
Query OK, 3 rows affected (0.00 sec)
Records: 3  Duplicates: 0  Warnings: 0
```



同时修改一下 tb_person 表新增一个 de_no 的字段如下所示：

```
mysql> alter table tb_person add column de_no int;
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc tb_person;
+---------+---------------+------+-----+---------+-------+
| Field   | Type          | Null | Key | Default | Extra |
+---------+---------------+------+-----+---------+-------+
| name    | varchar(50)   | NO   |     | NULL    |       |
| address | varchar(50)   | NO   |     | NULL    |       |
| salary  | decimal(10,2) | NO   |     | NULL    |       |
| de_no   | int(11)       | YES  |     | NULL    |       |
+---------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

mysql> update tb_person set de_no=1 where name='赵六';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update tb_person set de_no=2 where name='李四';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0

mysql> update tb_person set de_no=3 where name='王五';
Query OK, 1 row affected (0.00 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

例如查询所有员工的名字以及所在的部门如下所示：

```
mysql> select person.name ,department.name from tb_person person,tb_department department where person.de_no=department.id;
+--------+--------------+
| name   | name         |
+--------+--------------+
| 赵六   | 技术部门     |
| 李四   | hr           |
| 王五   | 产品线       |
+--------+--------------+
3 rows in set (0.00 sec)
```

上述SQL查询也等价于下列查询如下所示：

```
mysql> select person.name ,department.name from tb_person person inner join tb_department department on person.de_no=department.id;
+--------+--------------+
| name   | name         |
+--------+--------------+
| 赵六   | 技术部门     |
| 李四   | hr           |
| 王五   | 产品线       |
+--------+--------------+
3 rows in set (0.00 sec)
```

外连接又分为左外连接与右外连接，其中左外连接包含所有左表的记录甚至右表没有和它匹配的记录如下所示：

```
mysql> select person.name ,department.name from tb_person person left join tb_department department on person.de_no=department.id;
+-----------+--------------+
| name      | name         |
+-----------+--------------+
| 赵六      | 技术部门     |
| 李四      | hr           |
| 王五      | 产品线       |
| 川建国    | NULL         |
| 蓬佩奥    | NULL         |
| 川建国    | NULL         |
+-----------+--------------+
6 rows in set (0.00 sec)
```

上述查询中查询了所有的人名，即使有些人并不存在合法的部门名称，右外连接查询所有的右表记录，即使左表没有与其相匹配的记录。



* 子查询

某些情况下，当进行查询的时候通常 where 后跟的条件是另一个 select 语句后的结果，这个时候就要用到子查询。

一般可以见到的子查询关键字有 `in`，`not in`,`=`，`!=`, `exists`, `not exists`等。

例如查询员工部门号在1, 2的所有人员信息如下所示：

```
mysql> select * from tb_person where de_no in (select id from tb_department where id<3);
+--------+---------+----------+-------+
| name   | address | salary   | de_no |
+--------+---------+----------+-------+
| 赵六   | 南京    | 10000.00 |     1 |
| 李四   | 北京    | 18000.00 |     2 |
+--------+---------+----------+-------+
2 rows in set (0.00 sec)
```



* 联合查询

有时候对多表查询会进行合集处理，这个时候就可以使用UNION、UNION ALL关键字实现这样的功能，其语法如下所示：

```
SELECT * FROM 表1 UNION ｜ UNION ALL SELECT * FROM 表二
```

> UNION 与 UNION ALL 的区别是前者会将合并的结果进行去重处理而后者将会将所有的记录都显示出来

例如将两张表的部门编号进行联合查询如下所示：

```
mysql> select de_no from tb_person union all select id from tb_department;
+-------+
| de_no |
+-------+
|     1 |
|     2 |
|     3 |
|  NULL |
|  NULL |
|  NULL |
|     1 |
|     2 |
|     3 |
+-------+
9 rows in set (0.00 sec)

mysql> select de_no from tb_person union  select id from tb_department;
+-------+
| de_no |
+-------+
|     1 |
|     2 |
|     3 |
|  NULL |
+-------+
4 rows in set (0.00 sec)
```



## 3.3 DCL

一般DCL语句管理系统权限使用例如创建一个 corearchi 用户，并且对 test 下所有数据库有着 select 与 insert 权限

```
mysql> grant select,insert on test.* to 'corearchi'@'localhost' identified by '1234@Haha';
Query OK, 0 rows affected(0.01 sec)
```

此时我们可以使用切换至 `corearchi` 用户登录并插入数据如下所示：

```
[root@donniegao ~]# mysql -u corearchi -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.29 MySQL Community Server (GPL)
Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.
Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
mysql> use test;

mysql> desc tb_person;
+---------+---------------+------+-----+---------+-------+
| Field   | Type          | Null | Key | Default | Extra |
+---------+---------------+------+-----+---------+-------+
| name    | varchar(50)   | NO   |     | NULL    |       |
| address | varchar(50)   | NO   |     | NULL    |       |
| salary  | decimal(10,2) | NO   |     | NULL    |       |
| de_no   | int(11)       | YES  |     | NULL    |       |
+---------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
mysql> insert into tb_person values('小米','北京','30000',3);
Query OK, 1 row affected (0.00 sec)
```

然后我们在使用 `root` 权限收回`corearchi` 的 `insert` 权限如下所示：

```
mysql> revoke insert on test.* from 'corearchi'@'localhost';
Query OK, 0 rows affected (0.01 sec)
```

然后重新登录`corearchi`用户再来尝试插入数据如下所示：

```
mysql> use test;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A
Database changed
mysql> insert into tb_person values('小米','北京','30000',3);
ERROR 1142 (42000): INSERT command denied to user 'corearchi'@'localhost' for table 'tb_person'
```

我们发现corearchi用户无法再次向tb_person中插入数据了，因为其权限已经被收回。



# 4. 数据类型

## 4.1 Mysql数值类型

### 4.1.1 整数类型

Mysql数值类型包括`整数`类型以及`浮点型`类型，其中整数类型有如下类型：

| 类型名称  | 解释说明              | 存储空间 |
| :--------- | :--------------------- | :-------- |
| TINYINT   | MYSQL中最小的整数类型 | 1个字节  |
| SMALLINT  | MYSQL中小的整数       | 2个字节  |
| MEDIUMINT | MYSQL中等大小整数     | 3个字节  |
| INT       | MYSQL中默认的整数类型 | 4个字节  |
| BIGINT    | MYSQL中最大的整数类型 | 8个字节  |



在上一篇文章中创建了 `tb_person` 的表对象，其表信息如下所示：

```
mysql> desc tb_person;
+---------+---------------+------+-----+---------+-------+
| Field   | Type          | Null | Key | Default | Extra |
+---------+---------------+------+-----+---------+-------+
| name    | varchar(50)   | NO   |     | NULL    |       |
| address | varchar(50)   | NO   |     | NULL    |       |
| salary  | decimal(10,2) | NO   |     | NULL    |       |
| de_no   | int(11)       | YES  |     | NULL    |       |
+---------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)
```

de_no字段数据类型为`int(11)`，这表示的是该数据类型指定显示的宽度为11，这里需要注意显示宽度和实际类型存储数据最大范围是无关的，指定宽度是让mysql最大显示的数字长度个数，当插入的数据长度小于指定宽度时2不足之处将会由空格填充。如果插入的数据大于显示的宽度但是只要没有超过该数据类型最大范围值，数值依然可以插入表中列。

> 显示宽度只是用于显示，不会限制取值范围和占用空间，如int(3)依然是占有4个字节的空间大小且最大值也不是999。



### 4.1.2 浮点型与定点数

mysql中小数部分由浮点数与定点数组成。其中浮点型数值有以下类型：

| 类型名称 | 解释说明       | 存储空间 |
| :-------- | :-------------- | :-------- |
| FLOATT   | 单精度小数类型 | 4个字节  |
| DOUBLE   | 双精度小数类型 | 8个字节  |

定点数类型只有一种

| 类型名称          | 解释说明                 | 存储空间  |
| :----------------- | :------------------------ | :--------- |
| DEC、DECIMAL(M,D) | 最大取值范围与DOUBLE相同 | M+2个字节 |



浮点数与定点数都可以使用 `(M,D)` 表示。其中 `(M,D)`意味着数值一共显示 M 个数字，其中 D 为位于小数点后面，一般 M 被称为精度， D 称为标度。



下面以创建一个测试表以简单比较上述三种小数类型的区别：

```
mysql> create table demo1(x float(5,2),y double(5,2), z decimal(5,2))
Query OK, 0 rows affected (0.01 sec)
```

在 x,y,z 分别插入数据 5.21如下所示

```
mysql> insert into demo1 values (5.21,5.21,5.21);
Query OK, 1 row affected (0.00 sec)

mysql> select * from demo1;
+------+------+------+
| x    | y    | z    |
+------+------+------+
| 5.21 | 5.21 | 5.21 |
+------+------+------+
1 row in set (0.00 sec)
```

可以看到数据都已经插入成功，我们再向列中同时插入数据 1.315 如下所示：

```
mysql> insert into demo1 values (1.315,1.315,1.315);
Query OK, 1 row affected, 1 warnings (0.01 sec)
```

上面插入数据时候提示了警告信息我们可以查看一下警告信息如下：

```
mysql> show warnings;
+-------+------+----------------------------------------+
| Level | Code | Message                                |
+-------+------+----------------------------------------+
| Note  | 1265 | Data truncated for column 'z' at row 1 |
+-------+------+----------------------------------------+
1 rows in set (0.00 sec)

mysql> select * from demo1;
+------+------+------+
| x    | y    | z    |
+------+------+------+
| 5.21 | 5.21 | 5.21 |
| 1.31 | 1.31 | 1.32 |
+------+------+------+
2 rows in set (0.01 sec)
```

可以看到的是上述X，Y存储数据时候进行了四舍五入没有出现警告信息，而z字段数值直接被截断。



我们将上述字段的精度与标度去掉然后再插入数据1.315如下所示：

```
mysql> alter table demo1 modify column x float;
Query OK, 0 rows affected (0.00 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table demo1 modify column y double;
Query OK, 0 rows affected (0.00 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> alter table demo1 modify column z decimal;
Query OK, 2 rows affected, 2 warnings (0.01 sec)
Records: 2  Duplicates: 0  Warnings: 2

mysql> desc demo1;
+-------+---------------+------+-----+---------+-------+
| Field | Type          | Null | Key | Default | Extra |
+-------+---------------+------+-----+---------+-------+
| x     | float         | YES  |     | NULL    |       |
| y     | double        | YES  |     | NULL    |       |
| z     | decimal(10,0) | YES  |     | NULL    |       |
+-------+---------------+------+-----+---------+-------+
3 rows in set (0.00 sec)

mysql> select * from demo1;
+-------+-------+------+
| x     | y     | z    |
+-------+-------+------+
|  5.21 |  5.21 |    5 |
|  1.31 |  1.31 |    1 |
| 1.315 | 1.315 |    1 |
+-------+-------+------+
3 rows in set (0.00 sec)
```

我们发现x，y都可以进行数值的正常插入但是z字段的小数点部分已经全部被截取。

> FLOAT与DOUBLE在不指定精度、标度时，会默认按照实际的精度进行数据插入，如果DECIMAL不指定精度则默认认为DECIMAL(10,0)，如果数值类型指定了精度与标度时，则会按照四舍五入后的结果插入表中。一般在实际开发中如账户余额等对精度要求较高的情况下，一般使用的是DECIMAL类型且精度为10，标度为2



### 4.1.3 BIT类型

BIT类型可以存储二进制数据，其默认位数为1，无法使用select关键字进行查询，但是可以通过bin()与hex()函数对数据值进行读取。下面以一个简单的例子演示其使用：

```
mysql> alter table demo1 add column w bit;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc demo1;
+-------+---------------+------+-----+---------+-------+
| Field | Type          | Null | Key | Default | Extra |
+-------+---------------+------+-----+---------+-------+
| x     | float         | YES  |     | NULL    |       |
| y     | double        | YES  |     | NULL    |       |
| z     | decimal(10,0) | YES  |     | NULL    |       |
| w     | bit(1)        | YES  |     | NULL    |       |
+-------+---------------+------+-----+---------+-------+
4 rows in set (0.00 sec)

mysql> insert into demo1(w) values(1);
Query OK, 1 row affected (0.00 sec)

mysql> select * from demo1;
+-------+-------+------+------+
| x     | y     | z    | w    |
+-------+-------+------+------+
|  5.21 |  5.21 |    5 | NULL |
|  1.31 |  1.31 |    1 | NULL |
| 1.315 | 1.315 |    1 | NULL |
|  NULL |  NULL | NULL |     |
+-------+-------+------+------+
4 rows in set (0.00 sec)
```

正如你上面看到的新增的一行数据w的字段为空什么都看不到，我们在使用bin函数进行查询：

```
mysql> select w, bin(w) from demo1;
+------+--------+
| w    | bin(w) |
+------+--------+
| NULL | NULL   |
| NULL | NULL   |
| NULL | NULL   |
|     | 1      |
+------+--------+
4 rows in set (0.00 sec)
```



## 4.2 时间类型

mysql中时间类型定义的比较多，下面列出了mysql中日期与时间类型。

| 类型名称  | 日期格式            | 存储空间 |
| :--------- | :------------------- | :-------- |
| YEAR      | YYYY                | 1个字节  |
| TIME      | HH:MM:SS            | 3个字节  |
| DATE      | YYYY-MM-DD          | 3个字节  |
| DATETIME  | YYYY-MM-DD HH:MM:SS | 8个字节  |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS | 4个字节  |

DATE,DATETIME,TIMESTAMP是mysql最常用的三种时间类型，下面以简单的例子演示其使用：

```
mysql> create table timedemo(d date, dt datetime,ts timestamp);
Query OK, 0 rows affected (0.01 sec)

mysql> insert into timedemo values(now(),now(),now());
Query OK, 1 row affected, 1 warning (0.00 sec)

mysql> show warnings;
+-------+------+---------------------------------------------------------------------+
| Level | Code | Message                                                             |
+-------+------+---------------------------------------------------------------------+
| Note  | 1292 | Incorrect date value: '2020-06-10 05:44:51' for column 'd' at row 1 |
+-------+------+---------------------------------------------------------------------+
1 row in set (0.01 sec)

mysql> select * from timedemo;
+------------+---------------------+---------------------+
| d          | dt                  | ts                  |
+------------+---------------------+---------------------+
| 2020-06-10 | 2020-06-10 05:44:51 | 2020-06-10 05:44:51 |
+------------+---------------------+---------------------+
1 row in set (0.00 sec)
```

在上面我们使用了 now 函数插入了当前的日期，其中 date 类型插入的数据给出了警告信息，但是数据还是能正常插入进表。我们查看表信息如下所示：

```
mysql> desc timedemo;
+-------+-----------+------+-----+-------------------+-----------------------------+
| Field | Type      | Null | Key | Default           | Extra                       |
+-------+-----------+------+-----+-------------------+-----------------------------+
| d     | date      | YES  |     | NULL              |                             |
| dt    | datetime  | YES  |     | NULL              |                             |
| ts    | timestamp | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
+-------+-----------+------+-----+-------------------+-----------------------------+
3 rows in set (0.00 sec)
```

可以发现系统自动为ts创建了默认值 `CURRENT_TIMESTAMP` ，并且设置了 `not null` 和`on update`  CURRENT_TIMESTAMP。那么我们像`ts`这个列插入null查看是否默认插入了当前系统时间如下：

```
mysql> insert into timedemo(ts) values(null);
Query OK, 1 row affected (0.00 sec)

mysql> select * from timedemo;
+------------+---------------------+---------------------+
| d          | dt                  | ts                  |
+------------+---------------------+---------------------+
| 2020-06-10 | 2020-06-10 05:44:51 | 2020-06-10 05:44:51 |
| NULL       | NULL                | 2020-06-12 22:19:38 |
+------------+---------------------+---------------------+
2 rows in set (0.01 sec)
```

可以很清晰看到 `ts`的列已经插入到了数据，这里需要注意的是mysql只会给第一个类型为TimeStamp设置默认值，如果有第二个类型为TimeStamp的列将不会设置为当前时间，我们新增一个列验证一下：

```
mysql> alter table timedemo add column ts1 timestamp ;
ERROR 1067 (42000): Invalid default value for 'ts1'
```

我们发现增加新的TimeStamp类型必须为其指定一个默认值，所以我们设置其默认为 `null` 如下：

```
mysql> alter table timedemo add column ts1 timestamp null;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc timedemo;
+-------+-----------+------+-----+-------------------+-----------------------------+
| Field | Type      | Null | Key | Default           | Extra                       |
+-------+-----------+------+-----+-------------------+-----------------------------+
| d     | date      | YES  |     | NULL              |                             |
| dt    | datetime  | YES  |     | NULL              |                             |
| ts    | timestamp | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| ts1   | timestamp | YES  |     | NULL              |                             |
+-------+-----------+------+-----+-------------------+-----------------------------+
4 rows in set (0.00 sec)

mysql> insert into timedemo(ts1) values(null);
Query OK, 1 row affected (0.00 sec)

mysql> select * from timedemo;
+------------+---------------------+---------------------+------+
| d          | dt                  | ts                  | ts1  |
+------------+---------------------+---------------------+------+
| 2020-06-10 | 2020-06-10 05:44:51 | 2020-06-10 05:44:51 | NULL |
| NULL       | NULL                | 2020-06-12 22:19:38 | NULL |
| NULL       | NULL                | 2020-06-12 22:56:36 | NULL |
+------------+---------------------+---------------------+------+
3 rows in set (0.00 sec)
```

开发中经常设置的列时间类型为 `datetime` 类型，而不是 `timestamp` 类型。其原因 有如下几点：

1. 首先 `timestamp` 支持的范围时间小，最大时间只能为2038年的某一天，而`datetime` 类型支持的时间范围广最大可以支持到9999年。
2. `timestamp` 类型的数据插入和查询受到当前时区的影响，如果是其他时区的人看到会有时差。
3. `timestamp` 类型受到不同mysql版本影响较大。



## 4.3 字符串类型

字符串类型可以存储字符串数据，也可以存储二进制数据。mysql支持两种字符类型的数据即：文本字符串和二进制字符串。其中文本字符串有 `CHAR`,`VARCHAR`,`TEXT`,`ENUM`,`SET`，下表中列出了mysql的文本字符串类型。

| **类型名称** | 解释说明                             | 存储空间                                    |
| :------------ | :------------------------------------ | :------------------------------------------- |
| CHAR(M)      | 固定长度的文本字符串类型             | 一个字符串对象可以有0个或多个SET成员        |
| VARCHAR(M)   | 变长的文本字符串类型                 | M为0-65535之间的整数，其中值长度为M+1个字节 |
| TINYTEXT     | 非常小的文本字符串类型               | 存储长度在0-255个字节                       |
| TEXT         | 小的文本字符串类型                   | 存储长度为0-65535个字节                     |
| MEDIUMTEXT   | 比TEXT大的文本字符串类型             | 存储长度为0-2^24个字节长度                  |
| LONGTEXT     | 非常大的文本字符串类型               | 可存储0-2^32个字节长度                      |
| ENUM         | 枚举类型                             | 1-2个字节，取决于枚举值数目最大65535        |
| SET          | 一个字符串对象可以有0个或多个SET成员 | 1，2，3，4 最多支持64个成员                 |



### 4.3.1 CHAR 与 VARCHAR

CHAR(M) 与VARCHAR(M) 两者都可以存储较短的字符串类型，它们唯一的区别就是：CHAR类型创建时候声明的长度为固定长度，当保存时候右侧会填充空格以达到固定长度的值大小。而VARCHAR的实际长度为实际存储空间长度加一。下面将以一个例子描述两者的区别如下所示：

* 创建表

```
mysql> create table vcdemo(a char(4), b varchar(4));
Query OK, 0 rows affected (0.01 sec)
```



* 插入数据

```
mysql> insert into vcdemo values('ab  ','ab  ');
Query OK, 1 row affected (0.00 sec)
```



* 使用length函数查看列值长度

```
mysql> select length(a),length(b) from vcdemo;
+-----------+-----------+
| length(a) | length(b) |
+-----------+-----------+
|         2 |         4 |
+-----------+-----------+
1 row in set (0.00 sec)
```

从查询结果可以看到 a 在保存 'ab' 时候将末尾的两个空格删除了，而 b 字段保留了末尾的两个空格。



### 4.3.2 TEXT类型

Text类型的数据可以存储大量文本字符串，插入数据不会删除尾部空格。一般前端文本框保存大量文本内容时候就可以使用此类型。



### 4.3.3 ENUM类型

ENUM是一个字符串类型，其值为表创建时候在列中规定的枚举值，其语法如下所示：

```
CREATE TABLE 表名 （列名 ENUM(值1,值2,值3....));
```

下面一个简单的案例演示其使用：

* 创建表

```
mysql> create table enumdemo(gender enum('男','女'));
Query OK, 0 rows affected (0.01 sec)
```



* 插入数据

```
mysql> insert into enumdemo values('男'),('女'),(null),('1');
Query OK, 4 rows affected (0.00 sec)
Records: 4  Duplicates: 0  Warnings: 0
```



* 查询数据

```
mysql> select * from enumdemo;
+--------+
| gender |
+--------+
| 男     |
| 女     |
| NULL   |
| 男     |
+--------+
4 rows in set (0.00 sec)
```

从上面的例子中可以看出，如果插入的数据不是枚举类型中的值也不是null将会默认插入枚举中的第一个值。



### 4.3.4 SET类型

SET是一个字符串对象，其与ENUM类型很相似。其语法如下所示：

```
CREATE TABLE 表名 (列名 SET(值1,值2,值3....)）
```

SET类型与ENUM类型的区别就是：SET类型可以一次选取多个成员而ENUM则最多只能选一个。此外如果插入SET字段中的列值有重复，mysql会自动将其去重并添加到mysql中。下面以一个简单的例子演示其使用：

* 创建表

```
mysql> create table setdemo( sets set('a','b','c','d'));
Query OK, 0 rows affected (0.01 sec)
```



* 插入数据

```
mysql> insert into setdemo values('a,b'),('b,b,c'),('c,b,a');
Query OK, 3 rows affected (0.00 sec)
```



* 查询数据

```
mysql> select * from setdemo;
+-------+
| sets  |
+-------+
| a,b   |
| b,c   |
| a,b,c |
+-------+
3 rows in set (0.00 sec)
```

从结果可以看到对于set类型如果插入数据重复则只取一个。与此同时插入了不按顺序的值，mysql会将其转换为自动顺序插入，如上述'c,b,a'插入后显示值为'a,b,c'。



## 4.4  二进制字符串

### 4.4.1 BINARY与VARBINARY

二进制字符串中最常见的就是BINARY，VARBINARY，其类型类似于CHAR、VARCHAR，不同的是这两个类型存储的是二进制字符串，其语法如下所示：

```
CREATE TABLE 表名 (列名 BINARY(M)| VARBINARY)
```

BINARY类型的长度是固定的，在指定M长度后如果存储长度不足M长度的，则会在存储内容后填充若干'\0'以将存储内容达到M长度。例如为指定列类型为BINARY(3)，当插入'a'时，存储的内容实际为'a\0\0'。



VARBINARY的长度是可变的，指定M长度后，其长度可以在0-M之间，例如为指定列数据类型为VARBINARY(3)，当插入'a'时候，其存储内容就是'a'。下面以一个简单的案例演示其使用：

* 创建表

```
mysql> create table binarydemo(a binary(3),b varbinary(3));
Query OK, 0 rows affected (0.01 sec)
```



* 插入数据

```
mysql> insert into binarydemo values('5','5');
Query OK, 1 row affected (0.00 sec)
```



* 查询数据

```
mysql> select length(a),length(b) from binarydemo;
+-----------+-----------+
| length(a) | length(b) |
+-----------+-----------+
|         3 |         1 |
+-----------+-----------+
1 row in set (0.00 sec)
```

可以看到当插入数据不满最大长度时候，BINARY会填充字符以达到最大长度。



### 4.4.2 BLOB类型


BLOB是一个大的二进制内容存储类型，其存储的是字节字符串，需注意其与TEXT的区别，TEXT存储的是非二进制字符串内容，BLOB分为如下几个类型。





| 类型名称     |  解释说明         | 存储内容                    |
| :--------- | :--------------- | :-------------------------- |
| TINYBLOB   | 非常小的BLOB类型 | 存储最大长度为255个字节     |
| BLOB       | 小的BLOB类型     | 存储最大的长度为65535个字节 |
| MEDIUMBLOB | 稍微小的BLOB类型 | 存储最大长度为2^24-1 个字节 |
| LONGBLOB   | 最大的BLOB类型   | 最大长度为2^ ^2-1个字节     |




## 4.5 JSON类型

mysql从5.7.8版本后就新增了JSON类型，之前存储JSON内容我们都是将其保存到VARCHAR或TEXT类型上。下面以一个简单的例子演示此种数据类型的使用：

* 创建表

```
mysql> create table jsondemo(content json);
Query OK, 0 rows affected (0.01 sec)
```



* 插入数据

```
mysql> insert into jsondemo values('{"age":15,"gender":"男","name":"小明"}');
Query OK, 1 row affected (0.00 sec)
```



* 查询数据

```
mysql> select * from jsondemo;
+------------------------------------------------+
| content                                        |
+------------------------------------------------+
| {"age": 15, "name": "小明", "gender": "男"}    |
+------------------------------------------------+
1 row in set (0.00 sec)
```



## 4.6 总结

MYSQL中提供了很多的数据类型，为了优化存储，提高数据库性能。需要我们清晰了解什么样的业务数据使用最精确的类型，下面将简单总结日常开发中对于数据类型的选择。

### 4.6.1 整数和浮点数

如果存储的数值不包括小数部分，则使用整数类型。一般开发中常常使用的整数类型为int，实际可以根据你的业务存储数值大小可选择合适的整型数值类型。

浮点数包括float和double类型，double类型比float类型高，如果对存储浮点型数精度要求较高的情况下可选择double类型。

### 4.6.2 浮点数和定点数

浮点数比定点数一个优势就是浮点数能存储更大的数，但是由于其精度都没有定点数高，所以日常开发中对于余额、转账金额等精度要求极高的数据存储时候，decimal更能满足我们日常业务需求而无需担心其运算出现的精度损失的问题。

### 4.6.3 日期和时间类型

开发中最常见存储的时间类型为timestamp与datetime类型，timestamp存储的时间上限为2038年且其值插入与当前地区有关，所以日常中更多使用的是datetime类型。

### 4.6.4 char 与 varchar

我们已经知道char是固定长度存储字符串而varchar是变长的，所以char相对于varchar会占据更多的空间，但是其处理速度比varchar速度快，这就是典型的空间换时间问题。实际开发可以根据业务选择适合的数据类型。

### 4.6.5 enum 与 set

这两个数据类型在开发中使用频率不是很高，enum是一个枚举类型，如果从多个值取出一个值就可以使用枚举。例如：性别字段适合定义枚举类型，因为每个人的性格只能是男、女之间的一个。set可以取多值。

### 4.6.6 blob 与 text

blob是存储二进制字符串，而text是存储非二进制的数据，一般我们使用blob存储图片、音屏等信息，而text存储文本文件。

### 4.6.7 binary 与 varbinary

与char 和 varchar类似，我们知道binary是固定长度的而varbinary是变长的，当存储内容不足的时候binary会为存储的数据后面加入’\0’的内容，所以其占据更大的空间但速度更快。
