0
# 高级处理

# 5.1 窗口函数

窗口函数也称为OLAP函数。OLAP 是 OnLine AnalyticalProcessing 的简称，意思是对数据库数据进行实时分析处理。

为了便于理解，称之为 窗口函数。常规的SELECT语句都是对整张表进行查询，而窗口函数可以让我们有选择的去某一部分数据进行汇总、计算和排序。

```sql
<窗口函数> OVER ([ PARTITION BY <列名> ]
                     [ ORDER BY <排序用列名> ])  
```

PARTITON BY 子句 可选参数，指示如何将查询行划分为组，类似于 GROUP BY 子句的分组功能，但是 PARTITION BY 子句并不具备 GROUP BY 子句的汇总功能，并不会改变原始表中记录的行数。

ORDER BY 子句 可选参数，指示如何对每个分区中的行进行排序，即决定窗口内，是按那种规则(字段)来排序的。

举个例子

```sql
SELECT product_id ,
       product_name ,
       product_type ,
       sale_price ,
       RANK() OVER (PARTITION BY product_type 
                        ORDER BY sale_price) as RANKING 
FROM product;
```

表示根据product_type分类并且根据sale_price排名

# 5.2 窗口函数种类

一般分为两类

一类是SUM,MAX,MIN等聚合函数

一类是RANK,DENSE_RANKE等排序函数

# 5.2.1 排序专用的窗口函数

RANK函数

计算排序时，如果存在相同位次的记录，则会跳过之后的位次。

例）有 3 条记录排在第 1 位时：1 位、1 位、1 位、4 位……

DENSE_RANK函数

同样是计算排序，即使存在相同位次的记录，也不会跳过之后的位次。

例）有 3 条记录排在第 1 位时：1 位、1 位、1 位、2 位……

ROW_NUMBER函数

赋予唯一的连续位次。

例）有 3 条记录排在第 1 位时：1 位、2 位、3 位、4 位

```sql
SELECT product_id ,
       product_name ,
       product_type ,
       sale_price ,
       RANK() OVER (ORDER BY sale_price) as RANKING ,
       DENSE_RANK() OVER (ORDER BY sale_price) as RANKING2 ,
       ROW_NUMBER () OVER (ORDER BY sale_price) as RANKING3 
FROM product
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/6eea80b4-0129-4556-8913-e051bf5d518a)

# 5.2.2 对窗口函数使用聚合函数

```sql
SELECT  product_id
       ,product_name
       ,sale_price
       ,SUM(sale_price) OVER (ORDER BY product_id) AS current_sum
       ,AVG(sale_price) OVER (ORDER BY product_id) AS current_avg  
  FROM product;  
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/c7c791b1-0cc5-45f1-a374-920cc41cadb8)

可知对窗口函数聚合是对当前行之前的所有行聚合

# 5.3 计算移动平均

在上面提到，聚合函数在窗口函数使用时，计算的是累积到当前行的所有的数据的聚合。 实际上，还可以指定更加详细的汇总范围。该汇总范围称为 框架 (frame)。

PRECEDING（“之前”）， 将框架指定为 “截止到之前 n 行”，加上自身行

FOLLOWING（“之后”）， 将框架指定为 “截止到之后 n 行”，加上自身行

```sql
SELECT  product_id
       ,product_name 
       ,sale_price
       ,SUM(sale_price) OVER (ORDER BY sale_price ) as current_sum 
       ,SUM(sale_price) OVER (ORDER BY sale_price ROWS 1 PRECEDING) AS PRECEDING_sum
  FROM product;
```

表示求本身一行和之前一行的值，如果再求之后一行要用BETWEEN ··· AND ···

```sql
SELECT  product_id
       ,product_name 
       ,sale_price
       ,SUM(sale_price) OVER (ORDER BY sale_price ) as current_sum 
       ,SUM(sale_price) OVER (ORDER BY sale_price ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS PRECEDING_sum
  FROM product;
```

## 5.4 GROUPING 运算符

### 5.4.1 ROLLUP计算合计以及小计

常规的GROUP BY我们只能查询到这些

```sql
SELECT  product_type
       ,SUM(sale_price) AS sum_price
  FROM product
 GROUP BY product_type
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/88f68f28-4848-47aa-b90a-a52e22a0b548)

这是因为这时如果我们想查看regist_date的值都为NULL，因为GROUP BY已经聚合了那几行，我们可以使用ROLLUP函数把小计展开

```sql
SELECT  product_type
       ,regist_date
       ,SUM(sale_price) AS sum_price
  FROM product
 GROUP BY product_type, regist_date WITH ROLLUP; 
```

## 5.5 存储过程和函数

