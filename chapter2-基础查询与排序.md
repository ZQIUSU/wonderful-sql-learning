# 基础查询与排序

# 这是我们初始的表

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/c5f1b18e-1afe-41cf-bf75-be48781d54dc)

## 1.select语句

### 1.1从表中选取数据

这个是普通查找

```sql
select <表名> from <列名>;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/275dea48-00c5-435e-991a-94396ceb47ea)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/65bf1680-d58d-43d1-b5d5-6a6e0484956e)

这个是带有条件的查找

```sql
select <表名> from <列名> where <条件> ;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/ca66d4ef-0ba9-475d-bf1d-9cc4afe559e9)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/58b9e2de-5a00-4c70-9d02-1a4cc06d1e2d)

select后边也可以跟表达式

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/13b52935-fb30-4255-944f-688ecbfbea8f)

表示找出sale_price并且×2 as 是可以理解为别称吧

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/d27678a5-ed3e-480e-ac4a-c68826cecd24)

### 作业(第一部分)

#### 2.1

编写一条SQL语句，从 product(商品) 表中选取出“登记日期(regist_date)在2009年4月28日之后”的商品，查询结果要包含 product name 和 regist_date 两列

答:

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/455656f6-5f8a-41fa-b3ac-856ec132c39b)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/41bb6c27-0887-47e1-8274-e7aea223a2c2)

#### 2.2

请说出对product 表执行如下3条SELECT语句时的返回结果

①

```sql
SELECT *
  FROM product
 WHERE purchase_price = NULL;
```

答:查找出所有的列，但是不会出现任何结果

②

```sql
SELECT *
  FROM product
 WHERE purchase_price <> NULL;
```

答:同样的查找出所有的列，没有任何结果

③

```sql
SELECT *
  FROM product
 WHERE product_name > NULL;
```

答:同样查找出所有的列不会出现任何结果，因为NULL不能用比较运算符，需要查找为NULL的要用is NULL 或者is not NULL

#### 2.3 

`2.2.3` 章节中的SELECT语句能够从 `product` 表中取出“销售单价（`sale_price`）比进货单价（`purchase_price`）高出500日元以上”的商品。请写出两条可以得到相同结果的`SELECT`语句。执行结果如下所示：

```sql
product_name | sale_price | purchase_price 
-------------+------------+------------
T恤衫        | 　 1000    | 500
运动T恤      |    4000    | 2800
高压锅       |    6800    | 5000
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/efbd5582-a627-4b09-a020-a4e1a22c85ef)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/647384ef-9202-4ce8-b724-ed274cf4f6d2)

#### 2.4

请写出一条SELECT语句，从 `product` 表中选取出满足“销售单价打九折之后利润高于 `100` 日元的办公用品和厨房用具”条件的记录。查询结果要包括 `product_name`列、`product_type` 列以及销售单价打九折之后的利润（别名设定为 `profit`）。

提示：销售单价打九折，可以通过 `sale_price` 列的值乘以0.9获得，利润可以通过该值减去 `purchase_price` 列的值获得。

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/868a0f5b-bbb8-4afb-986c-bed302dbb07c)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/5754025e-d849-43cf-b108-df728b949016)

## 3 对表进行聚合查询

### 3.1 聚合函数

SQL中用于汇总的函数叫做聚合函数。以下五个是最常用的聚合函数：

- SUM：计算表中某数值列中的合计值

- AVG：计算表中某数值列中的平均值

- MAX：计算表中任意列中数据的最大值，包括文本类型和数字类型

- MIN：计算表中任意列中数据的最小值，包括文本类型和数字类型

- COUNT：计算表中的记录条数（行数）

```sql
-- 计算销售单价和进货单价的合计值
SELECT SUM(sale_price), SUM(purchase_price) 
  FROM product;
-- 计算销售单价和进货单价的平均值
SELECT AVG(sale_price), AVG(purchase_price)
  FROM product;
-- 计算销售单价的最大值和最小值
SELECT MAX(sale_price), MIN(sale_price)
  FROM product;
-- MAX和MIN也可用于非数值型数据
SELECT MAX(regist_date), MIN(regist_date)
  FROM product;
-- 计算全部数据的行数（包含 NULL 所在行）
SELECT COUNT(*)
  FROM product;
-- 计算 NULL 以外数据的行数
SELECT COUNT(purchase_price)
  FROM product;
```

### 使用DISTINCT删除重复行

对表进行聚合查询有时候会出现重复出现的行，我们要使用DISTINCT来删除相同的行

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/b2726257-5c63-480c-aa2a-9490b4fd17d3)

表示表格里product_type的种类有3种，如果没有distinct，会算上重复的种类

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/77ba4636-544c-4c92-9cb5-45f2e8262983)
