# 集合运算

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
