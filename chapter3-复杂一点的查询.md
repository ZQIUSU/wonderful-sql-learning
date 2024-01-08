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


