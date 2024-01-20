![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/4f00e322-3b29-417f-b963-104cf4b4335a)![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/53b4217d-7454-4b31-9961-db055828e5a0)# 集合运算

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

#### 4.2.1.2 结合WHERE语句使用內连结

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

#### 4.2.1.3 结合 GROUP BY 子句使用内连结

结合 GROUP BY 子句使用内连结，需要根据分组列位于哪个表区别对待。

最简单的情形，是在内连结之前就使用 GROUP BY 子句。

但是如果分组列和被聚合的列不在同一张表，且二者都未被用于连结两张表，则只能先连结，再聚合。

# 练习题

每个商店中，售价最高的商品的售价分别是多少？

```sql
SELECT sp.shop_name ,max(sale_price)
FROM product p 
INNER JOIN shopproduct sp 
ON p.product_id =sp.product_id 
GROUP BY sp.shop_name
```

# 思考题

上述查询得到了每个商品售价最高的商品，但并不知道售价最高的商品是哪一个。如何获取每个商店里售价最高的商品的名称和售价？

>注: 这道题的一个简易的方式是使用下一章的窗口函数。当然，也可以使用其他我们已经学过的知识来实现。例如，在找出每个商店售价最高商品的价格后，使用这个价格再与 product 列进行连结，但这种做法在价格不唯一时会出现问题。

#### 4.2.1.4 自连结

之前的内连结，连结的都是不一样的两个表。但实际上一张表也可以与自身作连结，这种连接称之为自连结。需要注意，自连结并不是区分于内连结和外连结的第三种连结，自连结可以是外连

结也可以是内连结，它是不同于内连结外连结的另一个连结的分类方法。

#### 4.2.1.5 内连结与关联子查询

回想一下之前的一个问题：求每个种类中售价高于平均价格的商品

当时是这么做的:

```sql
SELECT product_type,product_name,sale_price
FROM product as p1
WHERE sale_price>(SELECT AVG(sale_price)
                  FROM product as p2
		  WHERE p1.product_type=p2.product_type
		  GROUP BY product_type)
```

我们现在使用內连结同样可以解决这个问题

```sql
SELECT product_id,product_name,sale_price,p2.avg_price
FROM product p1 
INNER JOIN (SELECT product_type,
	    AVG(sale_price) as avg_price
            FROM product
            GROUP BY product_type) as p2
ON p.product_type=p2.product_type
WHERE p1.sale_price>p2.avg_price;
```

#### 4.2.1.6 自然连结

自然连结并不是区别于内连结和外连结的第三种连结，它其实是内连结的一种特例--当两个表进行自然连结时，会按照两个表中都包含的列名来进行等值内连结，此时无需使用 ON 来指定连接

条件。

```sql
SELECT * FROM shopproduct NATURAL JOIN product;
```

# 练习题 

试写出与上题等价的內连结

```sql
SELECT p.*,s.shop_id,s.shop_name,s.quantity
FROM product p 
INNER JOIN shopproduct s 
ON p.product_id =s.product_id 
```

使用自然连结还可以求出两张表或子查询的公共部分，例如教材中 7-1 选取表中公共部分--INTERSECT 一节中的问题：求表 product 和表 product2 中的公共部分，也可以用自然连结来实

现：

```sql
SELECT * FROM product NATURAL JOIN product2;
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/f5d7345d-a9f4-447f-8779-c797fc48bdfc)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/fa087c0d-254a-49d1-81c6-84a0079f9bfe)

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/16f8e2b9-741e-48dc-a56a-adeea9315d9a)

可以看到0003号没有连结上，这是因为0003号商品的regist_date字段为NULL，这是因为NATURAL是逐字段等值来进行连结的，NULL不等于NULL所以没有连结

### 4.2.2 外连结

内连结会丢弃两张表中不满足 ON 条件的行，和内连结相对的就是外连结。外连结会根据外连结的种类有选择地保留无法匹配到的行。

按照保留的行位于哪张表,外连结有三种形式：左连结，右连结和全外连结。

三种外连结的对应语法分别为：

```sql
-- 左连结     
FROM <tb_1> LEFT  OUTER JOIN <tb_2> ON <condition(s)>
-- 右连结     
FROM <tb_1> RIGHT OUTER JOIN <tb_2> ON <condition(s)>
-- 全外连结
FROM <tb_1> FULL  OUTER JOIN <tb_2> ON <condition(s)>
```

#### 4.2.2.1 左连结

练习题：统计每种商品分别在哪些商店有售，需要包括那些在每个商店都没货的商品。

```sql
SELECT SP.shop_id
       ,SP.shop_name
       ,SP.product_id
       ,P.product_name
       ,P.sale_price
  FROM product AS P
  LEFT OUTER JOIN shopproduct AS SP
    ON SP.product_id = P.product_id;
```

外连结还有一点非常重要，那就是要把哪张表作为主表。最终的结果中会包含主表内所有的数据。指定主表的关键字是 LEFT 和 RIGHT。

#### 4.2.2.2 使用WHERE子句使用左连结
 
# 练习题

使用外连结从shopproduct表和product表中找出那些在某个商店库存少于50的商品及对应的商店。希望得到如下结果。

```sql
SELECT  S.product_name,
	SP.shop_name,
FROM product as S
LEFT OUTER JOIN shopproduct as SP
ON SP.product_id=P.product_id
WHERE SP.quantity<50;
```

上边这个查询结果没有quantity为NULL的商品，很明显不符合我们要查找的结果，于是改进后

```sql
SELECT  P.product_name,
	SP.shop_name,
	SP.quantity 
FROM product as P
LEFT OUTER JOIN 
	(SELECT *
	FROM shopproduct 
	WHERE quantity<50) as SP
ON P.product_id=SP.product_id;
```

或者

```sql
SELECT  P.product_name,
	SP.shop_name,
	SP.quantity 
FROM product as P
LEFT OUTER JOIN shopproduct as SP
ON SP.product_id=P.product_id
WHERE SP.quantity<50
   OR quantity is NULL;
```

#### 4.2.2.3 在MYSQL中使用全外连结

有了对左连结和右连结的了解，就不难理解全外连结的含义了。全外连结本质上就是对左表和右表的所有行都予以保留，能用 ON 关联到的就把左表和右表的内容在一行内显示，不能被关联到

的就分别显示，然后把多余的列用缺失值填充。

遗憾的是，MySQL8.0 目前还不支持全外连结，不过我们可以对左连结和右连结的结果进行 UNION 来实现全外连结。、

### 4.2.3 多表连结

#### 4.2.3.1 多表內连结

先创建一个用于多表內连结的仓库库存管理表Inventoryproduct，假设有两个仓库P001,P002

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/4e94a4b3-64ad-4c23-b9e4-db55cf91fb06)

接下来，我们根据上表及 shopproduct 表和 product 表，使用内连接找出每个商店都有那些商品，每种商品的库存总量分别是多少。

```sql
SELECT SP.product_id ,
       SP.shop_id ,
       SP.shop_name ,
       P.product_name ,
       P.sale_price ,
       IP.quantity 
FROM shopproduct as SP
INNER JOIN product as P
ON SP.product_id =P.product_id 
INNER JOIN Inventoryproduct as IP
ON SP.product_id =IP.product_id 
WHERE IP.inventory_id ='P001';
```

得到如下结果

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/2422db0f-0a3d-474d-9aff-cbd5dcbe0e53)

#### 4.2.3.2 多表进行外连结

```sql
SELECT P.product_id ,
	   P.product_name ,
       P.sale_price ,
       SP.shop_id ,
       SP.shop_name ,
       IP.quantity 
FROM product as P
LEFT OUTER JOIN shopproduct as SP
ON SP.product_id =P.product_id 
LEFT OUTER JOIN Inventoryproduct as IP
ON SP.product_id =IP.product_id 
;
```

查询结果

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/6ef4e9b8-2fa8-44ad-a14b-a8bb9cc851a9)

### 4.2.4 ON子句进阶，非等值连结

#### 4.2.4.1 非等值自左连结

# 练习题

希望对 product 表中的商品按照售价赋予排名。一个从集合论出发，使用自左连结的思路是，对每一种商品，找出售价不低于它的所有商品，然后对售价不低于它的商品使用 COUNT 函数计

数。例如，对于价格最高的商品，

```sql
SELECT  product_id
       ,product_name
       ,sale_price
       ,COUNT(p2_id) AS my_rank
FROM  -- 使用自左连结对每种商品找出价格不低于它的商品
        (SELECT P1.product_id
               ,P1.product_name
               ,P1.sale_price
               ,P2.product_id AS P2_id
               ,P2.product_name AS P2_name
               ,P2.sale_price AS P2_price 
          FROM product AS P1 
          LEFT OUTER JOIN product AS P2 
            ON P1.sale_price <= P2.sale_price 
            ORDER BY sale_price DESC
        ) AS X
GROUP BY product_id, sale_price
ORDER BY my_rank; 
```

#### 4.2.4.2 多表进行外连结

正如之前所学发现的，外连结一般能比内连结有更多的行，从而能够比内连结给出更多关于主表的信息，多表连结的时候使用外连结也有同样的作用。

```sql
SELECT P.product_id
       ,P.product_name
       ,P.sale_price
       ,SP.shop_id
       ,SP.shop_name
       ,IP.inventory_quantity
  FROM product AS P
  LEFT OUTER JOIN shopproduct AS SP
ON SP.product_id = P.product_id
LEFT OUTER JOIN Inventoryproduct AS IP
ON SP.product_id = IP.product_id;
```

### 4.2.4 ON子句进阶--非等值连结

```sql
SELECT  product_id
       ,product_name
       ,sale_price
       ,COUNT(p2_id) AS my_rank
  FROM ( -- 使用自左连结对每种商品找出价格不低于它的商品
        SELECT P1.product_id
               ,P1.product_name
               ,P1.sale_price
               ,P2.product_id AS P2_id
               ,P2.product_name AS P2_name
               ,P2.sale_price AS P2_price 
          FROM product AS P1 
          LEFT OUTER JOIN product AS P2 
            ON P1.sale_price <= P2.sale_price 
            ORDER BY sale_price DESC
        ) AS X
 GROUP BY product_id, sale_price
 ORDER BY my_rank;
```

上边这个语句表示使用聚合函数找出每个商品有几个商品低于或等于自己的价格的，也就是按价格排序

注 1：COUNT 函数的参数是列名时，会忽略该列中的缺失值，参数为 * 时则不忽略缺失值。 注 2：上述排名方案存在一些问题--如果两个商品的价格相等，则会导致两个商品的排名错误，

例如，叉子和打孔器的排名应该都是第六，但上述查询导致二者排名都是第七。试修改上述查询使得二者的排名均为第六。

# 练习题

请按照商品的售价从低到高，对售价进行累计求和[注：这个案例缺少实际意义, 并且由于有两种商品价格相同导致了不必要的复杂度, 但示例数据库的表结构比较简单, 暂时未想出有实际意

义的例题]

```sql
SELECT product_id,
       product_name,
       sale_price,
       SUM(p2_price) as sum_price
  FROM (SELECT  P1.product_id
       ,P1.product_name
       ,P1.sale_price
       ,P2.product_id AS P2_id
       ,P2.product_name AS P2_name
       ,P2.sale_price AS P2_price 
  FROM product AS P1 
  LEFT OUTER JOIN product AS P2 
    ON P1.sale_price >= P2.sale_price
 ORDER BY P1.sale_price,P1.product_id) AS P3
 GROUP BY P3.product_id
 ORDER BY sum_price ASC;
```

注 1：COUNT 函数的参数是列名时，会忽略该列中的缺失值，参数为 * 时则不忽略缺失值。 注 2：上述排名方案存在一些问题--如果两个商品的价格相等，则会导致两个商品的排名错误，

例如，叉子和打孔器的排名应该都是第六，但上述查询导致二者排名都是第七。试修改上述查询使得二者的排名均为第六。注 3：实际上，进行排名有专门的函数，这是 MySQL 8.0 新增加的

窗口函数中的一种(窗口函数将在下一章学习)，但在较低版本的 MySQL 中只能使用上述自左连结的思路。

# 练习题

请按照商品的售价从低到高，对售价进行累计求和[注：这个案例缺少实际意义, 并且由于有两种商品价格相同导致了不必要的复杂度, 但示例数据库的表结构比较简单, 暂时未想出有实际意

义的例题]

首先找出比该商品售价更低的商品

```sql
SELECT product_id,
	   product_name,
	   sale_price,
       SUM(p2_price) as sum_price
FROM (SELECT p.product_id,
	       p.product_name,
	       p.sale_price,
	       p2.product_id as p2_id,
	       p2.product_name as p2_name,
	       p2.sale_price as p2_price
	FROM product p
	LEFT OUTER JOIN product p2 
	ON p.sale_price>=p2.sale_price 
	ORDER BY p.sale_price) as X
GROUP BY product_id
ORDER BY sum_price
```

![image](https://github.com/ZQIUSU/wonderful-sql-learning/assets/91874269/15e254be-18a5-4a47-9ae5-5efccc21b2b1)

我们可以发现售价相等时打孔器和叉子的和是相等的，所以我们需要改一下连结条件，找出低于p的售价的或者，等于p的售价的并且已经连结过一次的，我们这样理解，2，6号商品售价相等

我们这样2，6商品的售价总和都是1100，是这样算的，2号商品的售价大于2，6，8号商品，6号商品的售价大于2，6，8号商品，其中2，6是相等的，所以我们就可以只求售价相等的条件

下，不求id的那个，如果三个售价相等，4，7，9，我们求4号商品的总价就不包括7，9，求7号不包括9，求9的时候都包括，这样就解决了重复的问题了

### 4.2.5 交叉连结
