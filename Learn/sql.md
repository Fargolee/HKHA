
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