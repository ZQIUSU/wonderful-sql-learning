![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/53b4217d-7454-4b31-9961-db055828e5a0)# 集合运算

## 4.1 表的加减法

在标准 SQL 中，分别对检索结果使用 `UNION`，`INTERSECT`，`EXCEPT` 来将检索结果进行并集、交集和差集运算，像`UNION`，`INTERSECT`，`EXCEPT`这种用来进行集合运算的运算符称为集合运算符。

以下的文氏图展示了几种集合的基本运算。

### 4.1.1 表的加法 UNION

product 

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/6df289a7-c483-47bd-8b79-a8e5a5e74557)

product2

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/6950e065-0d53-42e7-88a2-8bfceb119a5b)

```sql
SELECT *
FROM product
UNION
SELECT *
FROM product2;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/2e6cd936-6659-4f25-acd4-0e38f439977a)

前提是列数要相同

# 练习题

假设连锁店想要增加成本利润率超过 50% 或者售价低于 800 的货物的存货量，请使用 UNION 对分别满足上述两个条件的商品的查询结果求并集。

```sql
SELECT *
FROM product 
WHERE sale_price > 1.5 * purchase_price 
UNION 
SELECT *
FROM product
WHERE sale_price <800;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/555cadf3-3449-46f3-bfe9-95e4b281056a)

### 4.1.2 UNION 和 OR

对于上边这道题，使用UNION和使用OR的结果是一样的，但是如果对不同的表使用合并就不得不使用UNION了

### 4.1.3 UNION ALL 不去重合并

我们可以发现UNION会去重,如果不想去重的话使用UNION ALL

### 4.1.4 隐式数据类型转换

通常来说, 我们会把类型完全一致，并且代表相同属性的列使用 UNION 合并到一起显示，但有时候，即使数据类型不完全相同，也会通过隐式类型转换来将两个类型不同的列放在一列里显示,

例如字符串和数值类型:

```sql
SELECT product_id, product_name, '1'
  FROM product
 UNION
SELECT product_id, product_name,sale_price
  FROM product2;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/2b52f153-bade-483b-bad9-aa85503eef2c)

证明字符串类型和数字类型可以兼容

### 4.1.3 INTERSECT 

对于不同的表求交集，MYSQL 8.0不支持INTERSECT，我们可以用INNER JOIN求交集

对于相同的表求不同条件的交集我们可以使用AND来完成操作

```sql
SELECT p1.product_id,p1.product_name
FROM product p1
INNER JOIN product2 p2
ON p1.product_id =p2.product_id;
```

### 4.1.4 差集，补集，表的减法

MYSQL 8.0不支持EXCEPT运算

找出属于product不属于product2的id

```sql
SELECT product_id
FROM product
WHERE product_id NOT IN(SELECT product_id
                         FROM product2);
```

### 4.1.5 对称差

对称差是指那些只属于A或者B的元素构成的集合

```sql
 SELECT *
 FROM product
 WHERE product_id NOT IN (SELECT product_id FROM product2)
 UNION
 SELECT *
 FROM product2
 WHERE product_id NOT IN (SELECT product_id FROM product);
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/2e56d743-9717-4745-a10d-b044c3803ea1)

## 4.2 JOIN 连结

连结(JOIN)就是使用某种关联条件(一般是使用相等判断谓词"=")， 将其他表中的列添加过来，进行“添加列”的集合运算。可以说，连结是 SQL 查询的核心操作，掌握了连结，能够从两张甚

至多张表中获取列，能够将过去使用关联子查询等过于复杂的查询简化为更加易读的形式，以及进行一些更加复杂的查询。

SQL 中的连结有多种分类方法，我们这里使用最基础的内连结和外连结的分类方法来分别进行讲解。

### 4.2.1 內连结

#### 4.2.1.1 使用內连结从两个表中获取信息

简单来说JOIN就是引申列，UNION，INTERACT，EXCEPT是对行做改变

```sql
FROM <table 1> INNER JOIN <table 2> ON <conditions>;
```

我们从shopproduct表和product表中可以获得一个公共信息就是product_id，作为连接两个表的桥梁

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/2f01cbb0-4d8b-4bf4-ba97-16281c873adc)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/1e1ee19f-5209-4a90-907b-44ea5cc0d356)

**要点一: 进行连结时需要在 FROM 子句中使用多张表**

之前的 FROM 子句中只有一张表，而这次我们同时使用了 shopproduct 和 product 两张表，使用关键字 INNER JOIN 就可以将两张表连结在一起了

**要点二:必须使用 ON 子句来指定连结条件**

在进行内连结时 ON 子句是必不可少的(大家可以试试去掉上述查询的 ON 子句后会有什么结果)。

ON 子句是专门用来指定连结条件的，我们在上述查询的 ON 之后指定两张表连结所使用的列以及比较条件，基本上，它能起到与 WHERE 相同的筛选作用，我们会在本章的结尾部分进一步探讨

这个话题。

**要点三: SELECT 子句中的列最好按照 表名.列名 的格式来使用**

当两张表的列除了用于关联的列之外，没有名称相同的列的时候，也可以不写表名，但表名使得我们能够在今后的任何时间阅读查询代码的时候，都能马上看出每一列来自于哪张表，能够节省

我们很多时间。

但是，如果两张表有其他名称相同的列，则必须使用上述格式来选择列名，否则查询语句会报错。

我们回到上述查询所回答的问题。通过观察上述查询的结果，我们发现，这个结果离我们的目标：找出东京商店的衣服类商品的基础信息已经很接近了。接下来，我们只需要把这个查询结果作

为一张表，给它增加一个 WHERE 子句来指定筛选条件。

### 4.2.1.2 结合WHERE语句使用內连结

WHERE语句放在ON语句的后边

子查询也是一个表，只不过是虚拟的，它并不真实存在于数据库中，只是数据库中其他表经过筛选，聚合等查询操作后得到的一个“视图”。

# 练习题

找出每个商店里的衣服类商品的名称及价格等信息，希望得到如下结果：

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/17f67d16-50a9-4d4a-8b31-a412ddf68b08)

```sql
SELECT sp.shop_id,
	   sp.shop_name,
	   sp.product_id,
	   p.product_name,
	   p.product_type,
	   p.purchase_price
FROM product p 
INNER JOIN shopproduct sp
ON p.product_id =sp.product_id 
WHERE product_type = '衣服';
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/ec14f4f9-f48f-42a0-8f7e-808ebbe357f5)

# 练习题

分别使用连结两个子查询和不使用子查询的方式，找出东京商店里，售价低于 2000 的商品信息，希望得到如下结果。

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/bca00b80-ce0d-4184-9db6-be97a5e8834b)

不使用子查询

```sql
SELECT p.*,
	   sp.*
FROM product p 
INNER JOIN shopproduct sp
ON p.product_id = sp.product_id 
WHERE sale_price <2000 AND sp.shop_name = '东京';
```

使用子查询

```sql
SELECT p.*,
	   sp.*
FROM (SELECT *
      FROM product 
      WHERE sale_price< 2000) as p  
INNER JOIN (SELECT * 
			FROM shopproduct 
		    WHERE shop_name = '东京') as sp
ON p.product_id = sp.product_id ;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/9257cac2-60e4-4844-8cf6-fd0724ef9859)
