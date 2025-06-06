
[SQL之母 - 免费SQL自学网站 by 程序员鱼皮](https://sqlmother.yupi.icu/#/levels)

在 SQL 中，我们可以使用 `DISTINCT` 关键字来实现去重操作

```sql
select distinct 字段1, 字段2, 字段3  from students
```



在查询数据时，我们有时希望对结果按照某个字段的值进行排序，以便更好地查看数据。
在 SQL 中，我们可以使用 `ORDER BY` 关键字来实现排序操作。`ORDER BY` 后面跟上需要排序的字段，可以选择升序（ASC）或降序（DESC）排列

select 字段 from 表 order by ```sql字段1 [升序/降序], 字段2 [升序/降序], ...


在 SQL 中，我们使用 `LIMIT` 关键字来实现数据的截断和偏移
```sql
-- LIMIT 后只跟一个整数，表示要截断的数据条数（一次获取几条）
select task_name, due_date from tasks limit 2;

-- LIMIT 后跟 2 个整数，依次表示从第几条数据开始、一次获取几条
select task_name, due_date from tasks limit 2, 2;
```

条件分支 `case when` 是 SQL 中用于根据条件进行分支处理的语法。它类似于其他编程语言中的 if else 条件判断语句，允许我们根据不同的条件选择不同的结果返回
```sql
CASE WHEN (条件1) THEN 结果1
	   WHEN (条件2) THEN 结果2
	   ...
	   ELSE 其他结果 END
```
```sql
SELECT
  name,
  CASE WHEN (name = '鸡哥') THEN '会' ELSE '不会' END AS can_rap
FROM
  student;
```

- DATE：获取当前日期
- DATETIME：获取当前日期时间
- TIME：获取当前时间
```sql
-- 获取当前日期
SELECT DATE() AS current_date;

-- 获取当前日期时间
SELECT DATETIME() AS current_datetime;

-- 获取当前时间
SELECT TIME() AS current_time;
```
|current_date|current_datetime|current_time|
|---|---|---|
|2023-08-01|2023-08-01 14:30:00|14:30:00|



1）使用字符串处理函数 `UPPER` 将姓名转换为大写：

```sql
-- 将姓名转换为大写
SELECT name, UPPER(name) AS upper_name
FROM employees;
```

2）使用字符串处理函数 `LENGTH` 计算姓名长度：

```sql
-- 计算姓名长度
SELECT name, LENGTH(name) AS name_length
FROM employees;

```
3）使用字符串处理函数 `LOWER` 将姓名转换为小写：

```sql
-- 将姓名转换为小写并进行条件筛选
SELECT name, LOWER(name) AS lower_name
FROM employees;
```


常见的聚合函数包括：

- COUNT：计算指定列的行数或非空值的数量。
- SUM：计算指定列的数值之和。
- AVG：计算指定列的数值平均值。
- MAX：找出指定列的最大值。
- MIN：找出指定列的最小值。
-聚合函数通常在 SELECT 语句中配合 GROUP BY 子句使用，用于对分组后的数据进行汇总分析
使用聚合函数 `COUNT(DISTINCT 列名)` 计算订单表中不同客户的数量：

```sql
SELECT COUNT(DISTINCT customer_id) AS customer_num
FROM orders;
```

在 SQL 中，通常使用 `GROUP BY` 关键字对数据进行分组。
```sql
SELECT customer_id, COUNT(order_id) AS order_num
FROM orders
GROUP BY customer_id;
```
```sql
-- 查询每个用户购买的每种商品的总金额，按照客户编号和商品编号分组
SELECT customer_id, product_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id, product_id;
```

HAVING 子句与条件查询 WHERE 子句的区别在于，WHERE 子句用于在 **分组之前** 进行过滤，而 HAVING 子句用于在 **分组之后** 进行过滤。

在 SQL 中，关联查询是一种用于联合多个数据表中的数据的查询方式。

其中，`CROSS JOIN` 是一种简单的关联查询，不需要任何条件来匹配行，它直接将左表的 **每一行** 与右表的 **每一行** 进行组合，返回的结果是两个表的笛卡尔积


INNER JOIN 只返回两个表中满足关联条件的交集部分，即在两个表中都存在的匹配行
```sql
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
JOIN departments d ON e.department = d.department;
```

在 SQL 中，OUTER JOIN 是一种关联查询方式，它根据指定的关联条件，将两个表中满足条件的行组合在一起，并 **包含没有匹配的行** 。

在 OUTER JOIN 中，包括 LEFT OUTER JOIN 和 RIGHT OUTER JOIN 两种类型，它们分别表示查询左表和右表的所有行（即使没有被匹配），再加上满足条件的交集部分
```sql
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
LEFT JOIN departments d ON e.department = d.department;
```

# 子查询
我们希望查询出订单总金额 > 200 的客户的姓名和他们的订单总金额，示例 SQL 如下：

```sql
-- 主查询
SELECT name, total_amount
FROM customers
WHERE customer_id IN (
    -- 子查询
    SELECT DISTINCT customer_id
    FROM orders
    WHERE total_amount > 200
);

```
# 子查询 - exists
子查询中的一种特殊类型是 "exists" 子查询，用于检查主查询的结果集是否存在满足条件的记录，它返回布尔值（True 或 False），而不返回实际的数据
```sql
-- 主查询
SELECT name, total_amount
FROM customers
WHERE EXISTS (
    -- 子查询
    SELECT 1
    FROM orders
    WHERE orders.customer_id = customers.customer_id
);
```
和 exists 相对的是 not exists，用于查找不满足存在条件的记录

# 组合查询
在 SQL 中，组合查询是一种将多个 SELECT 查询结果合并在一起的查询操作。

包括两种常见的组合查询操作：UNION 和 UNION ALL。

1. UNION 操作：它用于将两个或多个查询的结果集合并， **并去除重复的行** 。即如果两个查询的结果有相同的行，则只保留一行。
    
2. UNION ALL 操作：它也用于将两个或多个查询的结果集合并， **但不去除重复的行** 。即如果两个查询的结果有相同的行，则全部保留
UNION 操作：

```sql
SELECT name, age, department
FROM table1
UNION
SELECT name, age, department
FROM table2;
```
UNION 操作的结果，去除了重复的行

UNION ALL 操作：

```sql
-- UNION ALL操作
SELECT name, age, department
FROM table1
UNION ALL
SELECT name, age, department
FROM table2;
```
保留了重复的行

# 开窗函数 - sum over

该函数用法为：

```sql
SUM(计算字段名) OVER (PARTITION BY 分组字段名)
```
