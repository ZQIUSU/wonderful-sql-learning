# 初始数据库

## 注意sql语句结尾一般都会有个;

## 1.数据库的创建

create 创建数据库

```sql
create database <数据库名>
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/f6ee892b-adc4-472a-b751-3f3a36531878)

### 在创建表之前要选择一个数据库进行使用 

use 使用数据库

```sql
use <数据库名>
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/de2c117a-c94f-482f-a1d2-21b52cae7fd3)

## 2. 表操作

### 2.1 创建表

create table 创建表

```sql
create table <表名>(
<列名1> <数据类型1> <约束>,
···
···
<该表的约束1>,<该表的约束2>
);
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/cbccc57f-684e-4ef9-8f69-fdeed9a8c43c)

### 2.2删除表

drop table 删除表(表和数据都删除掉)

```sql
DORP table <表名>;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/1fece1f1-6d81-4a5f-83aa-8d9d1fed80ee)

truncate table 删除表中数据

```sql
TRUNCATE table product;
```

### 2.3改变表的结构

alter 可以添加或者删除列

```sql
ALTER table <表名> drop column
```

delete 可以删除行 

```sql
DELETE FROM <表名> where ;
```

### 2.4更新表数据

update 更新表数据

```sql
UPDATE <表名>
   SET <列名1> = <表达式1> , <列名2> = <表达式2> , ··· ···
 WHERE <条件>
ORDER BY
LIMIT;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/5961767d-1787-472e-83d8-d7a722fb647b)

### 2.5插入数据

insert into 插入数据

```sql
INSERT INTO <表名> (列1，列2，列3···) VALUES (值1，值2，值3···);
```

每个列都要插入时，可以省略列括号的内容

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/21f8c150-7091-45ff-b02f-359ae319dac6)

### 2.6索引

创建表时就可以创建索引

其时间复杂度为 ![1](http://latex.codecogs.com/svg.latex?log_2(N))

```sql
CREATE TABLE <表名>(
···
···
INDEX [indexname](username(length))
);
```

也可以用下列语句创建

create 直接创建索引

```sql
CREATE INDEX indexName ON table_name (column_name);
```

alter 为表中添加一个索引

```
ALTER table tableName ADD INDEX indexName(columnName);
```

- 索引分类
    - 主键索引
    
    建立在主键上的索引被称为主键索引，一张数据表只能有一个主键索引，索引列值不允许有空值，通常在创建表时一起创建。
    
    - 唯一索引
    
    建立在UNIQUE字段上的索引被称为唯一索引，一张表可以有多个唯一索引，索引列值允许为空，列值中出现多个空值不会发生重复冲突。
    
    - 普通索引
    
    建立在普通字段上的索引被称为普通索引。
    
    - 前缀索引
    
    前缀索引是指对字符类型字段的前几个字符或对二进制类型字段的前几个bytes建立的索引，而不是在整个字段上建索引。前缀索引可以建立在类型为char、varchar、binary、varbinary的列上，可以大大减少索引占用的存储空间，也能提升索引的查询效率。
    
    - 全文索引
    
    利用“分词技术”实现在长文本中搜索关键字的一种索引。
    
    语法：`SELECT * FROM article WHERE MATCH (col1，col2，...) AGAINST (expr [ search _ modifier ])`



# 课后作业

## 1.编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），并为 regist_no （注册编号）列设置主键约束

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/9386dc20-1618-481d-8173-7a486203964d)

答案: 

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/0631a659-85f1-450f-8df0-0b02d7bd1400)

## 2.假设在创建练习1.1中的 Addressbook 表时忘记添加如下一列 postal_code （邮政编码）了，请编写 SQL 把此列添加到 Addressbook 表中。

列名 ： postal_code饰 ：postal_code

数据类型 ：定长字符串类型（长度为 8）

约束 ：不能为 NULL

答案:

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/6ac1b1e9-23fd-4346-8722-f55bf9fb9799)

## 3.请补充如下 SQL 语句来删除 Addressbook 表。

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/0417694e-7d5c-44cc-9390-0d10eb39ccce)

答案:drop table addressbook;

## 4.是否可以编写 SQL 语句来恢复删除掉的 Addressbook 表？

答案:不可以
