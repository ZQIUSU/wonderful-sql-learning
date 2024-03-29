·# 基础查询与排序

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

SQL中用于汇总的函数叫做聚合函数。以下五个是最常用的聚合函数：(COUNT会处理所有行，包括NULL行，别的函数不会)

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

### 3.2 对表进行分组

#### 3.2.1 GROUP BY语句

GROUP BY进行分组汇总

```sql
SELECT <列名1>···
FROM <表名>
GROUP BY <列名1>···;
```

GROUP BY与聚合函数的差异:聚合函数是对整个表进行整合

#### 3.2.2 对取出的分组进行筛选

比如根据产品种类分为3组

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/20d5489e-6ddf-4f3c-8397-a5191469a836)

我该如何取出“衣服”和“办公用品”组类呢

在这里我们用HAVING函数

```sql
SELECT <列名1>···
FROM <表名>
GROUP BY <列名1>···
HAVING <条件>;
```

这里HAVING的用法和where的用法一样不过不能用where

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/fe52ab38-320e-4212-a020-ef4fd41a2c06)

这个例子表示的就是把购买价格大于2500的商品根据购买价格分类，count表示这一类有几个商品

#### 3.2.3 对表进行排序

##### ORDER BY

ASC表示升序，DESC表示降序，默认为ASC

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/bcdd69eb-3e07-459a-9002-24cf6d8c23f0)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/d3ddfcb6-9db9-43af-a3b1-670ed8a2e3e7)

表明在order by中是可以使用别名的，在sql语句中，执行顺序如下

FROM → WHERE → GROUP BY → SELECT → HAVING → ORDER BY 

这也是为什么GROUP BY中使用不了别名的原因

还有就是任何非NULL值都比NULL值大

### 作业(第二部分)

#### 4.1

请指出下述SELECT语句中所有的语法错误。

```sql
SELECT product_id, SUM（product_name）
--本SELECT语句中存在错误。
  FROM product 
 GROUP BY product_type 
 WHERE regist_date > '2009-09-01';
```

答:

1.要根据GROUP BY对product_type进行分组，SELECT 里肯定要有product_type

2.product_name 为string类型，不可以使用SUM聚合函数，可以用COUNT

3.GROUP BY后边不可以跟WHERE语句，筛选条件的话WHERE得在GROUP BY前边

下列是更改过后的语句

```sql
SELECT product_type,COUNT(product_name)
-- 在SELECT语句中使用了错误的列名，我将其更正为正确的列名product_type。
FROM product
WHERE regist_date > '2009-09-01'
GROUP BY product_type;
```

#### 4.2 

请编写一条SELECT语句，求出销售单价（ `sale_price` 列）合计值大于进货单价（ `purchase_price` 列）合计值1.5倍的商品种类。执行结果如下所示。

```sql
product_type | sum  | sum 
-------------+------+------
衣服         | 5000 | 3300
办公用品      |  600 | 320
```

答:

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/d40b39cb-9bab-404b-888b-402608e6395d)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/34bcc95e-b27b-4424-b6a6-8510ac6934bf)

#### 4.3

此前我们曾经使用SELECT语句选取出了product（商品）表中的全部记录。当时我们使用了 `ORDER BY` 子句来指定排列顺序，但现在已经无法记起当时如何指定的了。请根据下列执行结果，思考 `ORDER BY` 子句的内容。

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/993030aa-02fd-490c-b9f0-e1ac3848c97c)

答:

由图可知，是根据注册时间决定的，而且最前面是NULL，说明是ASC排序，而且在相同的时间段内，购买价格也是有规律的，而且最前面的也是NULL，说明和购买价格也有关系，而且也是ASC排序的，具体观察后，注册时间是降序，所以前面加负号，价格是升序，所以不用加，于是就是ORDER BY -regist_date,purchase_price;

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/3f962034-62d5-4b3d-9c3b-f6a3c5192739)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/32730908-de3c-4c39-bd38-ba6917befe7a)
