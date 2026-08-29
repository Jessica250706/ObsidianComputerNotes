# 概念

数据库，英文名称为Database，简称DB。从字面意思来看就是存储数据的仓库；从专业角度解释为存储在计算机磁盘上的有组织、可共享的大量数据的集合。

![image 29.png](images/image%2029.png)

## 类型

数据库分为关系型数据库和非关系型数据库两大类。常见的关系型数据库有  
Server 、SQLite 、DB2 等。常见的非关系型数据库有MySQL 、Oracle 、SQLRedis 、MongoDB 等。  

## DBMS/数据库管理系统

数据库管理系统，英文名称为Database Management System，简称DBMS。主要用于科学组织和存储数据、高效的获取和维护数据。

Eg. Navicat

## 连接

找到MySQL 安装目录下的bin 目录，然后打开命令窗口，在命令窗口中按如下语法输入命令：

```SQL
mysql -h MySQL数据库服务器的IP地址 -u 用户名 -p
```

然后按下回车键，输入密码即可。

# 结构化查询语言

## SQL分类

结构化查询语句，英文名称为Structured Query Language，简称SQL 。结构化查询语句分为数据定义语言、数据操作语言、数据查询语言和数据控制语言四大类。

|名称|描述|命令|
|---|---|---|
|数据定义语言/DDL|数据库、数据表的创建、修改和删除|CREATE、ALTER、DROP|
|数据操作语言/DML|数据的增加、修改和删除|INSERT、UPDATE、DELETE|
|数据查询语言/DQL|数据的查询|SELECT|
|数据控制语言/DCL|用户授权、事务的提交和回滚|GRANT、COMMIT、ROLLBACK|

## 操作

### 创建数据库

```SQL
CREATE DATABASE [IF NOT EXISTS] 数据库名称 DEFAULT CHARACTER SET 字符集 COLLATE 排序规则;
```

Eg. 创建数据库lesson，并指定字符集为GBK ，排序规则为GBK_CHINESE_CI

```SQL
CREATE DATABASE IF NOT EXISTS lesson DEFAULT CHARACTER SET GBK COLLATE GBK_CHINESE_CI;
```

### 修改数据库

```SQL
ALTER DATABASE 数据库名称 CHARACTER SET 字符集 COLLATE 排序规则;
```

Eg. ALTER DATABASE 数据库名称 CHARACTER SET 字符集 COLLATE 排序规则;

```SQL
ALTER DATABASE lesson CHARACTER SET UTF8 COLLATE UTF8_GENERAL_CI;
```

### 删除数据库

```SQL
DROP DATABASE [IF EXISTS] 数据库名称;
```

Eg. 删除数据库lesson

```SQL
DROP DATABASE IF EXISTS lesson;
```

### 查看数据库

```SQL
DROP DATABASE IF EXISTS lesson;
```

### 使用数据库

```SQL
USE 数据库名称;
```

## 列类型

在MySQL中，常用列类型主要分为数值类型、日期时间类型、字符串类型。

### 数据类型

| 类型        | 说明        | 取值范围                                           | 存储需求 |
| --------- | --------- | ---------------------------------------------- | ---- |
| tinyint   | 非常小的数据    | 有符号值： -2^7 ~ 2^7-1  <br>无符号值：0 ~ 2^8-1         | 1字节  |
| smallint  | 较小的数据     | 有符号值： -2^15 ~ 2^15-1  <br>无符号值： 0 ~ 2^16-1     | 2字节  |
| mediumint | 中等大小的数据   | 有符号值： -2^23 ~ 2^23-1  <br>无符号值： 0 ~ 2^24-1     | 3字节  |
| int       | 标准整数      | 有符号值： -2^31 ~ 2^31-1  <br>无符号值： 0 ~ 2^32-1     | 4字节  |
| bigint    | 较大的整数     | 有符号值： -2^63 ~ 2^63-1  <br>无符号值： 0 ~ 2^64-1     | 8字节  |
| float     | 单精度浮点数    | 无符号值：1.1754351 * 10^-38~3.402823466 *10^38     | 4字节  |
| double    | 双精度浮点数    | 无符号值：2.22507385 * 10^-308 ~ 1.79769313* 10^308 | 8字节  |
| decimal   | 字符串形式的浮点数 | decimal(m, d)                                  | m个字节 |

### 日期时间类型

|类型|说明|取值范围|
|---|---|---|
|DATE|YYYY-MM-dd ，日期格式|1000-01-01 ~ 9999-12-31|
|TIME|HH:mm:ss ，时间格式|1000-01-01 ~9999-12-31|
|DATETIME|YY-MM-dd HH:mm:ss|1000-01-01 00:00:00.000000 ~ 9999-12-31 23:59:59.999999|
|TIMESTAMP|YYYY-MM-dd HH:mm:ss 格式表示的时间戳|1970-01-01 00:00:01.000000 ~ 2038-01-19 03:14:07.999999|
|YEAR|YYYY格式的年份值|1901~2155|

### 字符串类型

|类型|说明|最大长度|
|---|---|---|
|char [(M)]|固定长字符串，检索快但费空间， 0 <= M <= 255|M字符|
|varchar [(M)]|可变字符串0 <= M <= 65535|变长度|
|text|文本串（存储长文本）|2^16–1字节|

### 列类型修饰属性

|属性名|说明|实例|
|---|---|---|
|UNSIGNED|无符号，只能修来修饰数值类型，表名该列数据不能出现负数|INT(4) UNSIGNED，表示只能为4位大于等于0的整数|
|ZEROFILL|不足的位数使用0来填充|INT(4) ZEROFILL，如果给定的值为10，此时只有2位，而该列需要4位，不足的2位由0来填充，最终值为0010|
|NOT NULL|表示该列类型的值不能为空|VARCHAR(20) NOT NULL，表示该列数据不能为空值|
|DEFAULT|表示设置默认值|INT(4) DEFAULT 0，表示该列不赋值时默认为0|
|AUTO_INCREMENT|表示自增长，只能应用于数值列类型，该列类型必须为键，且不能为空|INT(11) AUTO_INCREMENT NOT NULL PRIMARY KEY。第一次为该列中插入值时为1，第二次为2|

## 数据表操作

### 数据表类型

MySQL中的数据表类型有许多，如MyISAM、InnoDB、HEAP、BOB、CSV等。其中最常用的就是MyISAM和InnoDB。

### MyISAM与InnoDB的区别

|名称|MyISAM|InnoDB|
|---|---|---|
|事务处理|不支持|支持|
|数据行锁定|不支持|支持|
|外键约束|不支持|支持|
|全文索引|支持|不支持|
|表空间大小|较小|较大，约2倍|

事务：涉及的所有操作是一个整体，要么都执行，要么都不执行。

数据行锁定：一行数据，当一个用户在修改该数据时，可以直接将该条数据锁定。

### 选择：如何选择数据表的类型?

当涉及的业务操作以查询居多，修改和删除较少时，可以使用MyISAM。

当涉及的业务操作经常会有修改和删除操作时，使用InnoDB。

### 创建数据表

```SQL
CREATE TABLE [IF NOT EXISTS] 数据表名称(
		字段名1 列类型(长度) [修饰属性] [键/索引] [注释],
		字段名2 列类型(长度) [修饰属性] [键/索引] [注释],
		字段名3 列类型(长度) [修饰属性] [键/索引] [注释],
		……
		字段名n 列类型(长度) [修饰属性] [键/索引] [注释]
 ) [ENGINE = 数据表类型][CHARSET=字符集编码] [COMMENT=注释];
```

Eg. 创建学生表，表中有字段学号、姓名、性别、年龄和成绩

```SQL
CREATE TABLE IF NOT EXISTS student(
		 `number` VARCHAR(30) NOT NULL PRIMARY KEY COMMENT '学号，主键',
		 name VARCHAR(30) NOT NULL COMMENT '姓名',
		 sex TINYINT(1) UNSIGNED DEFAULT 0 COMMENT '性别：0-男 1-女 2-其他',
		 age TINYINT(3) UNSIGNED DEFAULT 0 COMMENT '年龄',
		 score DOUBLE(5, 2) UNSIGNED COMMENT '成绩'
 )ENGINE=InnoDB CHARSET=UTF8 COMMENT='学生表';
```

### 修改数据表

- 修改表名
    
    ```SQL
    ALTER TABLE 表名 RENAME AS 新表名;
    ```
    
    Eg. 将student表名称修改为stu
    
    ```SQL
    ALTER TABLE student RENAME AS stu;
    ```
    
- 增加字段
    
    ```SQL
    ALTER TABLE 表名 ADD 字段名 列类型(长度) [修饰属性] [键/索引] [注释];
    ```
    
    Eg. 在stu表中添加字段联系电话(phone)，类型为字符串，长度为11，非空
    
    ```SQL
    ALTER TABLE stu ADD phone VARCHAR(11) NOT NULL COMMENT '联系电话';
    ```
    
- 查看表结构
    
    ```SQL
    DESC 表名; -- 查看表结构
    ```
    
- 修改字段
    
    ```SQL
    -- MODIFY 只能修改字段的修饰属性
    ALTER TABLE 表名 MODIFY 字段名 列类型(长度) [修饰属性] [键/索引] [注释];
    
    -- CHANGE 可以修改字段的名字以及修饰属性
    ALTER TABLE 表名 CHANGE 字段名 新字段名 列类型(长度) [修饰属性] [键/索引] [注释];
    ```
    
    Eg. 将stu 表中的sex 字段的类型设置为VARCHAR ，长度为2，默认值为'男'，注释为 "性别，男，女，其他"。
    
    ```SQL
    ALTER TABLE stu MODIFY sex VARCHAR(2) DEFAULT '男' COMMENT '性别：男，女，其他';
    ```
    
    Eg. 将stu表中phone 字段修改为mobile ，属性保持不变。
    
    ```SQL
    ALTER TABLE stu CHANGE phone mobile VARCHAR(11) NOT NULL COMMENT '联系电话';
    ```
    
- 删除字段
    
    ```SQL
    ALTER TABLE 表名 DROP 字段名;
    ```
    
    Eg. 将stu表中的mobile 字段删除
    
    ```SQL
    ALTER TABLE  stu DROP mobile;
    ```
    

### 删除数据表

```SQL
DROP TABLE [IF EXISTS] 表名;
```

Eg. 删除数据表stu

```SQL
DROP TABLE IF EXISTS stu;
```