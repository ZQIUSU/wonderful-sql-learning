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

```sql
ALTER table <表名> drop

```sql
ALTER table <表名> drop column
```

DELETE 可以删除行 

```sql
DELETE FROM <表名> where 
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

### 2.5插入数据

insert into 插入数据

```sql
INSERT INTO <表名> (列1，列2，列3···) VALUES (值1，值2，值3···);
```

每个列都要插入时，可以省略列括号的内容

