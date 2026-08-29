# 定义

DQL全称是Data Query Language，表示数据查询语言。体现在数据的查询操作上，因此，DQL仅包括SELECT语句。

# SELECT语句

```SQL
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件
```

## 解释说明

ALL表示查询所有满足条件的记录，可以省略；

DISTINCT表示去掉查询结果中重复的记录；

AS可以给数据列、数据表取一个别名；

## 举例

```SQL
SELECT name FROM course WHERE `number`<5;
```

```SQL
SELECT score, `time` FROM course WHERE name='Java基础';
```

```SQL
# 查询
# 列的别名
SELECT score AS '学分', `time` as '学时' FROM course WHERE name='Java基础';
# 表的别名
SELECT c.score, c.time FROM course c WHERE c.name='Java基础';
```

## 比较操作符

|操作符|语法|说明|
|---|---|---|
|IS NULL|字段名 IS NULL|如果字段的值为NULL，则条件满足|
|IS NOT NULL|字段名 IS NOT NULL|如果字段的值不为NULL，则条件满足|
|BETWEEN … AND|字段名 BETWEEN 最小值 AND 最大值|如果字段的值在最小值与最大值之间（能够取到最小值和最大值），则条件满足|
|LIKE|字段名 LIKE '%匹配内容%’|如果字段值包含有匹配内容，则条件满足|
|IN|字段名 IN(值1，值2，...， 值n)|如果字段值在值1,值2, ...，值n中，则条件满足|

```SQL
SELECT * FROM course WHERE name IS NULL;
```

```SQL
SELECT * FROM course WHERE name IS NOT NULL;
```

```SQL
SELECT * FROM course WHERE score BETWEEN 2 AND 4;
```

```SQL
SELECT * FROM course WHERE name LIKE '%v%';
```

```SQL
SELECT * FROM course WHERE name LIKE 'J%';
```

```SQL
SELECT * FROM course WHERE name LIKE '%p';
```

```SQL
SELECT * FROM course WHERE name LIKE '__p';
```

```SQL
SELECT * FROM course WHERE `number` IN (1, 3, 5);
```

## 分组

### 数据表准备

```SQL
# 删除表
DROP TABLE IF EXISTS student;

# 新建表
CREATE TABLE student(
  no BIGINT(20) AUTO_INCREMENT NOT NULL PRIMARY KEY COMMENT '学号，主键',
  name VARCHAR(20) NOT NULL COMMENT '姓名',
  sex VARCHAR(2) DEFAULT '男' COMMENT '性别',
  age INT(3) DEFAULT 0 COMMENT '年龄',
  score DOUBLE(5, 2) COMMENT '成绩'
)ENGINE=InnoDB CHARSET=UTF8 COMMENT='学生表';

# 新增数据
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '张三', '男', 20, 59);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '李四', '女', 19, 62);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '王五', '其他', 21, 62);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '龙华', '男', 22, 75);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '金凤', '女', 18, 80);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '张华', '其他', 27, 88);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '李刚', '男', 30, 88);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '潘玉明', '女', 28, 81);
INSERT INTO student(no, name, sex, age, score) VALUES (DEFAULT, '凤飞飞', '其他', 32, 90);
```

### 分组查询

```SQL
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 GROUP BY 字段名1，字段名2,..., 字段名n
```

p.s. 分组查询所得的结果只是该组中的第一条数据。

```SQL
# 关闭 ONLY_FULL_GROUP_BY 模式
SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```

```SQL
SELECT * FROM student WHERE score>80 GROUP BY sex;
```

![[image 31.png|image 31.png]]

```SQL
SELECT * FROM student WHERE score BETWEEN 60 AND 80 GROUP BY sex, age;
```

![[image 1 11.png|image 1 11.png]]

### 聚合函数

- COUNT()
    
    说明：统计满足条件的数据总条数
    
    ```SQL
    SELECT COUNT(*) total FROM student WHERE score>80;
    ```
    
- SUM()
    
    说明：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的总和
    
    ```SQL
    SELECT COUNT(*) totalCount, SUM(score) totalScore FROM student WHERE score<60;
    ```
    
- AVG()
    
    说明：：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的平均值
    
    ```SQL
    SELECT sex, AVG(score) avgScore FROM student GROUP BY sex;
    ```
    
- MAX()
    
    说明：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的最大值
    
    ```SQL
    SELECT MAX(age) FROM student;
    ```
    
- MIN()
    
    说明：只能用于数值类型的字段或者表达式，计算该满足条件的字段值的最小值
    
    ```SQL
    SELECT MIN(score) FROM student;
    ```
    

### 分组查询结果筛选

```SQL
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 GROUP BY 字段名1，字段名2,..., 字段名n HAVING 筛选条件
```

分组后如果还需要满足其他条件，则需要使用HAVING子句来完成。

```SQL
SELECT * FROM student WHERE age BETWEEN 20 AND 30 GROUP BY sex HAVING avg(score)>74;
```

## 排序

```SQL
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 ORDER BY 字段名1 ASC|DESC，字段名2 ASC|DESC,..., 字段名n ASC|DESC
```

p.s. ORDER BY 必须位于 WHERE 条件之后。

p.ss. 默认是ASC，即正序；DESC为降序；

### 举例

```SQL
SELECT * FROM student WHERE age BETWEEN 18 AND 30 ORDER BY score DESC, age ASC;
```

![[image 2 10.png|image 2 10.png]]

## 分页

```SQL
SELECT ALL/DISTINCT * | 字段名1 AS 别名1[,字段名1 AS 别名1, ..., 字段名n AS 别名n] FROM 表名 WHERE 查询条件 LIMIT 偏移量, 查询条数
```

LIMIT的第一个参数表示偏移量，也就是跳过的行数。

LIMIT的第二个参数表示查询返回的最大行数，可能没有给定的数量那么多行。

### 举例

```SQL

# 分页
SELECT * FROM student WHERE score>=60 LIMIT 0, 3; # 第一个参数表示偏移量，也就是跳过的行数
SELECT * FROM student WHERE score>=60 LIMIT 3, 3; # 目标指令
SELECT * FROM student WHERE score>=60 LIMIT 6, 3;
```

![[image 3 7.png|image 3 7.png]]

![[image 4 6.png|image 4 6.png]]

![[image 5 6.png|image 5 6.png]]

p.s. 如果一个查询中包含分组、排序和分页，那么它们之间必须按照**分组->排序->分页**的先后顺序排列。