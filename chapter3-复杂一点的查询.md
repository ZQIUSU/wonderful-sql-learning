# 第三章 复杂一点的查询

## 3.1 视图

### 3.1.1 什么是视图

视图是一个虚拟的表，不同于直接操作数据表，视图是依据SELECT语句来创建的（会在下面具体介绍），所以操作视图时会根据创建视图的SELECT语句生成一张虚拟表，然后在这张虚拟表上做SQL操作。

### 3.1.2 视图与表有什么区别

视图不是表，视图是虚表，视图依赖于表

### 3.1.3 已经有表了，为什么还需要视图

1.通过定义视图可以将频繁使用的SELECT语句保存以提高效率。

2.通过定义视图可以使用户看到的数据更加清晰。

3.通过定义视图可以不对外公开数据表全部字段，增强数据的保密性。

4.通过定义视图可以降低数据的冗余。

### 3.1.4 如何创建视图

```sql
CREATE VIEW <视图名称>(<列名1>,<列名2>,...) AS <SELECT语句>
```

注意，视图的名称是唯一的，不能与其他表或视图重名

我们也可以在视图的基础上再创建视图，不仅仅是在表的基础上创建

虽然在视图上继续创建视图的语法没有错误，但是我们还是应该尽量避免这种操作。这是因为对多数 DBMS 来说， 多重视图会降低 SQL 的性能。

- 基于单表的视图

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/b2183d6e-093b-418e-989f-fff2e047983b)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/b5ac2670-9154-4bb7-ba40-5b71c468ce1e)

- 基于多表的视图

为学习多表视图，再创建一个表shop_product

创建的视图如下图所示

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/72a09fdc-7187-4368-8bd6-3bbc92c8f76f)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/4a50e074-fe91-42ac-8e06-6cb67bff4d1d)

我们来查询一个语句

```sql
SELECT sale_price,shop_name
FROM view_shop_product
WHERE product_type="衣服";
```

查询结果为:

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/3181204c-eeff-496d-bee6-89d181110e70)

### 3.1.5 如何修改视图结构

```sql
ALTER VIEW <视图名称> AS <SELECT语句>;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/cbf78df6-11cd-4034-8419-f0f671e8198a)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/d0b765f0-710e-4d1f-9ef0-cd64e0d6deca)

### 3.1.6 如何更新视图内容

因为视图是一个虚拟表，所以对视图的操作就是对底层基础表的操作，所以在修改时只有满足底层基本表的定义才能成功修改。

对于一个视图来说，如果包含以下结构的任意一种都是不可以被更新的：

* 聚合函数 SUM()、MIN()、MAX()、COUNT() 等。
  
* DISTINCT 关键字。

* GROUP BY 子句。

* HAVING 子句。

* UNION 或 UNION ALL 运算符。

* FROM 子句中包含多个表。

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/c5873b36-e95c-4600-b617-6fd44e1a927d)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/9d57f9c7-2321-4d90-b680-2ce5716fc609)

视图是由表派生出来的，所以视图更新的时候表也会更新，如下图

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/9b3df93d-6896-4427-9e59-6368f427a0a4)

我们可以看到，只有一个product_type="办公用品"的商品改变了，由此我们可以得知视图相当于一个窗口，所以修改也只能修改通过窗口看到的内容

**注意：这里虽然修改成功了，但是并不推荐这种使用方式。而且我们在创建视图时也尽量使用限制不允许通过视图来修改表**

### 3.1.7 如何删除视图

```sql
DROP VIEW <视图名称>;
```

## 3.2 子查询

### 3.2.1 什么是子查询

子查询指一个查询语句嵌套在另一个查询语句内部的查询，这个特性从 MySQL 4.1 开始引入，在 SELECT 子句中先计算子查询，子查询结果作为外层另一个查询的过滤条件，查询可以基于一个表或者多个表。

### 3.2.2 子查询与视图的关系

子查询和创建视图的SELECT语句一样，只不过子查询不会像视图那样保存，SELECT语句执行之后就消失了

### 3.2.3 标量子查询

标量就是单一的意思，那么标量子查询也就是单一的子查询

要求SQL语句返回一个值，具体到某一行某一列

### 3.2.4 标量子查询有什么用

```sql
SELECT product_id, product_name, sale_price
  FROM product
 WHERE sale_price > (SELECT AVG(sale_price) FROM product);
```

标量子查询不仅仅局限于 WHERE 子句中，通常任何可以使用单一值的位置都可以使用。也就是说， 能够使用常数或者列名的地方，无论是 SELECT 子句、GROUP BY 子句、HAVING 子句，还是 ORDER BY 子句，几乎所有的地方都可以使用。

我们还可以这样使用标量子查询

```sql
SELECT product_id,
       product_name,
       sale_price,
       (SELECT AVG(sale_price)
          FROM product) AS avg_price
  FROM product;
```

### 3.2.5 关联子查询

- 关联子查询与子查询的关系

我们之前的例子，`查询出销售单价高于平均销售单价的商品`，他的语句是这样写的

```sql
SELECT product_id,product_name,sale_price
FROM product
WHERE sale_price > (SELECT AVG(sale_price)
                      FROM product);
```

我们再来看一下接下来这个例子，`选取出各商品种类中高于该商品种类的平均销售单价的商品`，SQL语句如下

```sql
SELECT product_id,product_name,sale_price
  FROM product as P1
WHERE sale_price > (SELECT AVG(sale_price)
                      FROM product as P2
                    WHERE P1.product_type=P2.product_type);
```

# 练习题

1.创建出满足下述三个条件的视图（视图名称为 ViewPractice5_1）。使用 product（商品）表作为参照表，假设表中包含初始状态的 8 行数据。

- 条件 1：销售单价大于等于 1000 日元。

- 条件 2：登记日期是 2009 年 9 月 20 日。

- 条件 3：包含商品名称、销售单价和登记日期三列。

```sql
CREATE VIEW ViewPractice5_1
AS
(SELECT product_name,sale_price,regist_date
FROM product 
WHERE sale_price >= 1000 and regist_date ="2009-09-11" );
```

2.向习题一中创建的视图`ViewPractice5_1`中插入如下数据，会得到什么样的结果？为什么？

```sql
INSERT INTO ViewPractice5_1 VALUES (' 刀子 ', 300, '2009-11-02');
```

答:会报错，具体不知道为什么

3.请根据如下结果编写 SELECT 语句，其中 sale_price_avg 列为全部商品的平均销售单价。

```sql

SELECT product_id,
	product_name,
	product_type,
	sale_price,
	(SELECT avg(sale_price)
		FROM product) as avg_sale_price
FROM product;
```

4.请根据习题一中的条件编写一条 SQL 语句，创建一幅包含如下数据的视图（名称为AvgPriceByType）。

```sql
CREATE VIEW AvgPriceByType
AS
(SELECT product_id,
	product_name,
	product_type,
	sale_price,
	(SELECT AVG(sale_price) 
	FROM product as P2
	WHERE P1.product_type=P2.product_type
	GROUP BY product_type) as sale_price_avg_type
FROM product as P1);
```

这里求各种商品分别种类的平均售价，我们用关联子查询
