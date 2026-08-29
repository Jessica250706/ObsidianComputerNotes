# 定义

DML全称为Data Manipulation Language，表示**数据操作语言**。主要体现于对表数据的增删改操作。因此DML仅包括INSERT、UPDATE和DELEETE语句。

# 语句

## INSERT语句

```SQL
-- 需要注意，VALUES后的字段值必须与表名后的字段名一一对应
INSERT INTO 表名(字段名1, 字段名2, ..., 字段名n) VALUES(字段值1, 字段值2, ..., 字段值n);

-- 需要注意，VALUES后的字段值必须与创建表时的字段顺序保持一一对应
INSERT INTO 表名 VALUES(字段值1, 字段值2, ..., 字段值n);

-- 一次性插入多条数据
INSERT INTO 表名(字段名1, 字段名2, ..., 字段名n) VALUES(字段值1, 字段值2, ..., 字段值n),(字段值1, 字段值2, ..., 字段值n), ... , (字段值1, 字段值2, ..., 字段值n);
INSERT INTO 表名 VALUES(字段值1, 字段值2, ..., 字段值n), (字段值1, 字段值2, ..., 字段值n), ..., (字段值1, 字段值2, ..., 字段值n);
```

### 举例：向课程表中插入数据

```SQL
# 创建表：课程
 CREATE TABLE IF NOT EXISTS stu_course(
		 `number` INT(11) AUTO_INCREMENT NOT NULL PRIMARY KEY COMMENT '课程编号',
		 name VARCHAR(30) NOT NULL COMMENT '课程名称',
		 score DOUBLE(5, 2) NOT NULL COMMENT '学分'
 )ENGINE=InnoDB CHARSET=UTF8 COMMENT='课程表';
 
 # 修改
 # 将课程表名称修改为course
 ALTER TABLE stu_course RENAME AS course;
 # 在课程表中添加字段学时（time），类型为整数，长度为3，非空
 ALTER TABLE course ADD `time` INT(3) NOT NULL COMMENT '学时';
 # 修改课程表学分类型为浮点数，小数点后面保留1位有效数字，长度为3，非空
 ALTER TABLE course MODIFY score DOUBLE(3, 1) NOT NULL COMMENT '学分';
```

```SQL
# 插入
# 写法一
INSERT INTO course(`number`, name, score, `time`) VALUES(1, 'Java基础', 4, 40);
# 写法二：省略
INSERT INTO course VALUES(2, '数据库', 3, 20);
# 写法三：多条
INSERT INTO course(`number`, name, score, `time`) VALUES(4, 'Java基础', 4, 40), (5, '数电', 5, 45);
```

![image 30.png](images/image%2030.png)

## UPDATE语句

```SQL
UPDATE 表名 SET 字段名1=字段值1[,字段名2=字段值2, ..., 字段名n=字段值n] [WHERE 修改条件]
```

### WHERE条件子句

在Java中，条件的表示通常都是使用关系运算符来表示，在SQL语句中也是一样，使用 >, <, >=, <=, != 来表示。不同的是，除此之外，SQL中还可以使用SQL专用的关键字来表示条件。这些将在后面的DQL语句中详细讲解。

在Java中，条件之间的衔接通常都是使用逻辑运算符来表示，在SQL语句中也是一样，但通常使用AND来表示逻辑与(&&)，使用OR来表示逻辑或(||)

```SQL
WHERE time > 20 && time < 40;  <=>  WHERE time > 20 and time <40;
```

### UPDATE语句

```SQL
UPDATE course SET score=4, `time`=15 WHERE name='数据库';
```

## DELETE语句

```SQL
DELETE FROM 表名 [WHERE 删除条件];
```

### 举例

```SQL
DELETE FROM course WHERE `number`=1;
```

## TRUNCATE语句

```SQL
-- 清空表中数据
TRUNCATE [TABLE] 表名;
```

### 举例

```SQL
TRUNCATE course;
```

## DELETE与TRUNCATE的区别

- DELETE语句根据条件删除表中数据，而TRUNCATE语句则是将表中数据全部清空；如果DELETE语句要删除表中所有数据，那么在效率上要低于TRUNCATE语句。
- 如果表中有自增长列，TRUNCATE语句会重置自增长的计数器，但DELETE语句不会。
- TRUNCATE语句执行后，数据无法恢复，而DELETE语句执行后，可以使用事务回滚进行恢复。

DML语句