---
updateTime: "2024-10-11 17:06"
desc: "sql 入门笔记用来复习的"
outline: deep
---
### 1.全表查询
```sql
select * from student
```
### 2. 选择字段查询
只查询需要的字段
```sql
select name, age  from student
```
### 3.字段重命名
as 重命名
```sql
select name as 学生姓名, age as 学生年龄 from student
```
### 4.常量和运算
```sql
// 注意这个200是常量可以直接写 b就是常量字段名
select name, score, 200 as b from student

// 字段与字段可以运算的 加减乘除都可以
select name, score, score * age as double_score from student

// 字段可以运算的
select name, score, score * 2 as double_score from student
```
### 5.条件查询where
where 是条件查询 可以使用比较运算符（如=、<、>等）、逻辑运算符（如AND、OR等）、IN 操作符、LIKE 操作符等来设置条件。
```sql
select name,score from student where name = '鱼皮'
select name,score from student where name != '鱼皮'
select name,score from student where age > 10 AND name = '鱼皮'
select name,score from student where age > 10 OR name = '鱼皮'
// 使用 "BETWEEN" 运算符筛选出年龄在 25 到 30 之间的员工
select name, age, salary from employees where age between 25 and 30;
```
### 6.判断字段是否null 
```sql
// 判断非null
select name, age, score from student where age IS NOT NULL

// 判断是null
select name, age, score from student where age IS NULL 
```

### 7.模糊查询
```sql
// 查询以李开头的名称
select name, score from student where name like '李%'
// 查询以李结尾的名称
select name, score from student where name like '%李'
// 查询有李的名称
select name, score from student where name like '%李%'

// 查询不以李字开头的名称
select name, score from student where name not like '李%'
// 查询不以李字结尾的名称
select name, score from student where name not like '%李'
// 查询没有李字的名称
select name, score from student where name not like '%李%'
```

### 8.条件查询 - 逻辑运算
1. AND：表示逻辑与，要求同时满足多个条件，才返回 true。
2. OR：表示逻辑或，要求满足其中任意一个条件，就返回 true。
3. NOT：表示逻辑非，用于否定一个条件（本来是 true，用了 not 后转为 false）
```sql
// 查询以李开头的名称**并且**年龄小于20岁的
select name, score from student where name like '%李' AND age < 20

// 查询以李开头的名称**或者**年龄小于20岁的
select name, score from student where name like '%李' OR age < 20

// 查询**不**以李开头的名称**或者**年龄小于20岁的
select name, score from student where name not like '%李' OR age < 20
```

### 9.去重
distinct 字段名会去重查询出来的结果
```sql
select distinct class_id, exam_num from student
```

### 10.排序
ORDER BY 字段名 把查询结果按这个字段数据排序回来 ，可以选择升序（ASC）或降序（DESC）排列。
```sql
-- 按年龄升序
select name, age from students order by age asc;

-- 按年龄降序
select name, score from students order by score desc;
```
在排序的基础上，我们还可以根据多个字段的值进行排序。当第一个字段的值相同时，再按照第二个字段的值进行排序，以此类推。
```sql
-- 按年龄升序, 如果年龄相同再按分数升序
select name, age from students order by age asc，score asc;
```

### 11.截断和偏移（分页）LIMIT
```sql
-- LIMIT 后只跟一个整数，表示要截断的数据条数（一次获取几条）
select task_name, due_date from tasks limit 2;

-- LIMIT 后跟 2 个整数，依次表示从第几条数据开始、一次获取几条
select task_name, due_date from tasks limit 2, 2;
```

### 12.条件分支
条件分支 case when 是 SQL 中用于根据条件进行分支处理的语法。它类似于其他编程语言中的 if else 条件判断语句
```sql
SELECT
  name,
  CASE WHEN (name = '鸡哥') THEN '会' ELSE '不会' END AS can_rap
FROM
  student;
```

### 13.时间函数
常用的时间函数有：
1. DATE：获取当前日期
2. DATETIME：获取当前日期时间
3. IME：获取当前时间
```sql
-- 获取当前日期
SELECT DATE() AS current_date;

-- 获取当前日期时间
SELECT DATETIME() AS current_datetime;

-- 获取当前时间
SELECT TIME() AS current_time;
```

### 14.字符串处理
常用的时间函数有：
1. UPPER 将字符串转换为大写：
2. LOWER 将字符串转换为小写：
3. LENGTH 计算字符串长度
```sql
-- 查询大写的姓名
select id, name, UPPER(name) as upper_name from student where name = '热dog' 

-- 查询小写的姓名
select id, name, LOWER(name) as upper_name from student where name = '热dog'

-- 查询姓名的长度
select id, name, LENGTH(name) as name_length from student where name = '热dog'
```

### 15.聚合函数
常用的时间函数有：
1. COUNT：计算指定列的行数或非空值的数量。
2. SUM：计算指定列的数值之和。
3. AVG：计算指定列的数值平均值。
4. MAX：找出指定列的最大值。
5. MIN：找出指定列的最小值。
```sql
-- 使用聚合函数 COUNT 计算订单表中的总订单数
SELECT COUNT(*) AS order_num

-- 汇总学生表中所有学生的总成绩（total_score）、平均成绩（avg_score）、最高成绩（max_score）和最低成绩（min_score）
select
  SUM(score) as total_score,
  AVG(score) as avg_score,
  max(score) as max_score,
  min(score) as min_score 
  from student
```

### 16.单字段分组 GROUP BY
```sql
-- 统计学生表中每个班级的平均成绩（avg_score）
select class_id, avg(score) as avg_score from student GROUP BY class_id
-- 可以分组多个字段
select class_id, exam_num, avg(score) as avg_score from student GROUP BY class_id, exam_num
```

### 17.having 子句
HAVING 子句与条件查询 WHERE 子句的区别在于，WHERE 子句用于在 分组之前 进行过滤，而 HAVING 子句用于在 分组之后 进行过滤。
```sql
-- 假设有一个学生表 student，包含以下字段：
-- id（学号）、name（姓名）、class_id（班级编号）、score（成绩）。
-- 请你编写一个 SQL 查询，统计学生表中班级的总成绩超过 
-- 150 分的班级编号（class_id）和总成绩（total_score）。
SELECT class_id, SUM(score) AS total_score 
FROM student GROUP BY class_id HAVING SUM(score) > 150;
```

### 18.关联查询 - cross join
CROSS JOIN 是一种简单的关联查询，不需要任何条件来匹配行，它直接将左表的 每一行 与右表的 每一行 进行组合，返回的结果是两个表的笛卡尔积。
就是左表有3条数据 右表有3条数据 用CROSS JOIN联表查询他们组成9条数据表返回来
```sql
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
CROSS JOIN departments d;
```


### 19.关联查询 - inner join
INNER JOIN 只返回两个表中满足关联条件的交集部分，即在两个表中都存在的匹配行。
```sql
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
JOIN departments d ON e.department = d.department;
```

### 20.关联查询 - outer join
在 SQL 中，OUTER JOIN 是一种关联查询方式，它根据指定的关联条件，将两个表中满足条件的行组合在一起，并 包含没有匹配的行 。

在 OUTER JOIN 中，包括 LEFT OUTER JOIN 和 RIGHT OUTER JOIN 两种类型，它们分别表示查询左表和右表的所有行（即使没有被匹配），再加上满足条件的交集部分。

假设有一个员工表 `employees`，包含以下字段：`emp_id`（员工编号）、`emp_name`（员工姓名）、`department`（所属部门）、`salary`（工资）。数据如下：

| emp_id | emp_name | department | salary |
| ------ | -------- | ---------- | ------ |
| 1      | 小明     | 技术部     | 5000   |
| 2      | 鸡哥     | 财务部     | 6000   |
| 3      | 李华     | 销售部     | 4500   |



假设还有一个部门表 `departments`，包含以下字段：`department`（部门名称）、`manager`（部门经理）、`location`（所在地）。数据如下：

| department | manager | location |
| ---------- | ------- | -------- |
| 技术部     | 张三    | 上海     |
| 财务部     | 李四    | 北京     |
| 人事部     | 王五    | 广州     |
| 摸鱼部     | 赵二    | 吐鲁番   |



使用 LEFT JOIN 进行关联查询，根据员工表和部门表之间的部门名称进行匹配，将员工的姓名、工资以及所属部门和部门经理组合在一起，并包含所有员工的信息：

```sql
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
LEFT JOIN departments d ON e.department = d.department;
```



查询结果：

| emp_name | salary | department | manager |
|----------|--------|------------|---------|
| 小明     | 5000   | 技术部     | 张三    |
| 鸡哥     | 6000   | 财务部     | 李四    |
| 李华     | 4500   | 销售部     | NULL    |

### 21.子查询
子查询是指在一个查询语句内部 嵌套 另一个完整的查询语句，内层查询被称为子查询。子查询可以用于获取更复杂的查询结果或者用于过滤数据。

当执行包含子查询的查询语句时，数据库引擎会首先执行子查询，然后将其结果作为条件或数据源来执行外层查询。
```sql
SELECT name, total_amount
FROM customers
WHERE customer_id IN (
    -- 子查询
    SELECT DISTINCT customer_id
    FROM orders
    WHERE total_amount > 200
);
```
### 22.子查询 - exists
之前的教程讲到，子查询是一种强大的查询工具，它可以嵌套在主查询中，帮助我们进行更复杂的条件过滤和数据检索。

其中，子查询中的一种特殊类型是 "exists" 子查询，用于检查主查询的结果集是否存在满足条件的记录，它返回布尔值（True 或 False），而不返回实际的数据。
```sql
-- 获取 不存在对应班级的 学生的所有数据，返回学生姓名（name）、年龄（age）、班级编号（class_id）字段。
select
  name,
  age,
  class_id
from
  student
where
  not exists (
    select
      1
    from
      class
    where
      class.id = student.class_id
  )
```

### 23.组合查询（分表的时候用到这个）
在 SQL 中，组合查询是一种将多个 SELECT 查询结果合并在一起的查询操作。

包括两种常见的组合查询操作：UNION 和 UNION ALL。

UNION 操作：它用于将两个或多个查询的结果集合并， 并去除重复的行 。即如果两个查询的结果有相同的行，则只保留一行。

UNION ALL 操作：它也用于将两个或多个查询的结果集合并， 但不去除重复的行 。即如果两个查询的结果有相同的行，则全部保留。
```sql
-- 假设有一个学生表 student，包含以下字段：id（学号）、name（姓名）、age（年龄）、
-- score（分数）、class_id（班级编号）。还有一个新学生表 student_new，包含的字段和学生表完全一致。

-- 请编写一条 SQL 语句，获取所有学生表和新学生表的学生姓名（name）、年龄（age）、分数（score）、
-- 班级编号（class_id）字段，要求保留重复的学生记录。
select
  name,
  age,
  score,
  class_id
from
  student
union all
select
  name,
  age,
  score,
  class_id
from
  student_new
```

### 24.开窗函数 - sum over
## 示例
假设我们有订单表 `orders`，表格数据如下：

| order_id | customer_id | order_date | total_amount |
|----------|-------------|------------|--------------|
| 1        | 101         | 2023-01-01 | 200          |
| 2        | 102         | 2023-01-05 | 350          |
| 3        | 101         | 2023-01-10 | 120          |
| 4        | 103         | 2023-01-15 | 500          |



现在，我们希望计算每个客户的订单总金额，并显示每个订单的详细信息。

示例 SQL 如下：

```sql
SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    SUM(total_amount) OVER (PARTITION BY customer_id) AS customer_total_amount
FROM
    orders;
```



查询结果：

| order_id | customer_id | order_date  | total_amount | customer_total_amount |
|----------|-------------|-------------|--------------|-----------------------|
| 1        | 101         | 2023-01-01  | 200          | 320                   |
| 3        | 101         | 2023-01-10  | 120          | 320                   |
| 2        | 102         | 2023-01-05  | 350          | 350                   |
| 4        | 103         | 2023-01-15  | 500          | 500                   |



在上面的示例中，我们使用开窗函数 SUM 来计算每个客户的订单总金额（customer_total_amount），并使用 PARTITION BY 子句按照customer_id 进行分组。从前两行可以看到，开窗函数保留了原始订单的详细信息，同时计算了每个客户的订单总金额。

### 25.开窗函数 - sum over order by
## 示例
假设我们有订单表 `orders`，表格数据如下：

| order_id | customer_id | order_date | total_amount |
|----------|-------------|------------|--------------|
| 1        | 101         | 2023-01-01 | 200          |
| 2        | 102         | 2023-01-05 | 350          |
| 3        | 101         | 2023-01-10 | 120          |
| 4        | 103         | 2023-01-15 | 500          |



现在，我们希望计算每个客户的历史订单累计金额，并显示每个订单的详细信息。

```sql
SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    SUM(total_amount) OVER (PARTITION BY customer_id ORDER BY order_date ASC) AS cumulative_total_amount
FROM
    orders;
```



结果将是：

| order_id | customer_id | order_date  | total_amount | cumulative_total_amount |
|----------|-------------|-------------|--------------|-------------------------|
| 1        | 101         | 2023-01-01  | 200          | 200                     |
| 3        | 101         | 2023-01-10  | 120          | 320                     |
| 2        | 102         | 2023-01-05  | 350          | 350                     |
| 4        | 103         | 2023-01-15  | 500          | 500                     |



在上面的示例中，我们使用开窗函数 SUM 来计算每个客户的历史订单累计金额（cumulative_total_amount），并使用 PARTITION BY 子句按照 customer_id 进行分组，并使用 ORDER BY 子句按照 order_date 进行排序。从结果的前两行可以看到，开窗函数保留了原始订单的详细信息，同时计算了每个客户的历史订单累计金额；相比于只用 sum over，同组内的累加列名称


### 26.开窗函数 - rank
## 示例
假设我们有订单表 `orders`，表格数据如下：

| order_id | customer_id | order_date | total_amount |
|----------|-------------|------------|--------------|
| 1        | 101         | 2023-01-01 | 200          |
| 2        | 102         | 2023-01-05 | 350          |
| 3        | 101         | 2023-01-10 | 120          |
| 4        | 103         | 2023-01-15 | 500          |



现在，我们希望为每个客户的订单按照订单金额降序排名，并显示每个订单的详细信息。

```sql
SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    RANK() OVER (PARTITION BY customer_id ORDER BY total_amount DESC) AS customer_rank
FROM
    orders;
```



查询结果：

| order_id | customer_id | order_date | total_amount | customer_rank |
| -------- | ----------- | ---------- | ------------ | ------------- |
| 1        | 101         | 2023-01-01 | 200          | 1             |
| 3        | 101         | 2023-01-10 | 120          | 2             |
| 2        | 102         | 2023-01-05 | 350          | 1             |
| 4        | 103         | 2023-01-15 | 500          | 1             |



在上面的示例中，我们使用开窗函数 RANK 来为每个客户的订单按照订单金额降序排名（customer_rank），并使用 PARTITION BY 子句按照 customer_id 进行分组，并使用 ORDER BY 子句按照 total_amount 从大到小进行排序。

可以看到，开窗函数保留了原始订单的详细信息，同时计算了每个客户的订单金额排名。


### 27.开窗函数 - row_number
## 示例
假设我们有订单表 `orders`，表格数据如下：

| order_id | customer_id | order_date | total_amount |
|----------|-------------|------------|--------------|
| 1        | 101         | 2023-01-01 | 200          |
| 2        | 102         | 2023-01-05 | 350          |
| 3        | 101         | 2023-01-10 | 120          |
| 4        | 103         | 2023-01-15 | 500          |



现在，我们希望为每个客户的订单按照订单金额降序排列，并且分配一个 row_number 编号，示例 SQL 语句如下：

```sql
SELECT 
    order_id, 
    customer_id, 
    order_date, 
    total_amount,
    ROW_NUMBER() OVER (PARTITION BY customer_id ORDER BY total_amount DESC) AS row_number
FROM
    orders;
```


结果将是：

| order_id | customer_id | order_date | total_amount | row_number |
| -------- | ----------- | ---------- | ------------ | ---------- |
| 4        | 103         | 2023-01-15 | 500          | 1          |
| 2        | 102         | 2023-01-05 | 350          | 1          |
| 1        | 101         | 2023-01-01 | 200          | 1          |
| 3        | 101         | 2023-01-10 | 120          | 2          |



在上面的示例中，我们使用开窗函数 ROW_NUMBER 为每个客户的订单按照订单金额降序排列，并为每个订单分配了一个编号（row_number），并使用 PARTITION BY 子句按照 customer_id 进行分组，并使用 ORDER BY 子句按照 total_amount 进行排序。




### 28.开窗函数 - lag / lead
## 示例

以下是一个示例，假设我们有一个学生成绩表`scores`，其中包含学生的成绩和考试日期：

| student_id | exam_date  | score |
| ---------- | ---------- | ----- |
| 101        | 2023-01-01 | 85    |
| 101        | 2023-01-05 | 78    |
| 101        | 2023-01-10 | 92    |
| 101        | 2023-01-15 | 80    |



现在我们想要查询每个学生的考试日期和上一次考试的成绩，以及下一次考试的成绩，示例 SQL 如下：

```sql
SELECT 
    student_id,
    exam_date,
    score,
    LAG(score, 1, NULL) OVER (PARTITION BY student_id ORDER BY exam_date) AS previous_score,
    LEAD(score, 1, NULL) OVER (PARTITION BY student_id ORDER BY exam_date) AS next_score
FROM
    scores;
```

结果将是：

| student_id | exam_date  | score | previous_score | next_score |
| ---------- | ---------- | ----- | -------------- | ---------- |
| 101        | 2023-01-01 | 85    | NULL           | 78         |
| 101        | 2023-01-05 | 78    | 85             | 92         |
| 101        | 2023-01-10 | 92    | 78             | 80         |
| 101        | 2023-01-15 | 80    | 92             | NULL       |

在上面的示例中，我们使用 Lag 函数获取每个学生的上一次考试成绩（previous_score），使用 Lead 函数获取每个学生的下一次考试成绩（next_score）。如果没有上一次或下一次考试，对应的列将显示为 NULL。

