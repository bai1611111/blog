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
### 3.常量和运算
```sql
// 注意这个200是常量可以直接写 b就是常量字段名
select name, score, 200 as b from student

// 字段与字段可以运算的 加减乘除都可以
select name, score, score * age as double_score from student

// 字段可以运算的
select name, score, score * 2 as double_score from student
```
### 3.条件查询where
where 是条件查询 可以使用比较运算符（如=、<、>等）、逻辑运算符（如AND、OR等）、IN 操作符、LIKE 操作符等来设置条件。
```sql
select name,score from student where name = '鱼皮'
select name,score from student where name != '鱼皮'
select name,score from student where age > 10 AND name = '鱼皮'
select name,score from student where age > 10 OR name = '鱼皮'
// 使用 "BETWEEN" 运算符筛选出年龄在 25 到 30 之间的员工
select name, age, salary from employees where age between 25 and 30;
```
### 3. 判断字段是否null 
```sql
// 判断非null
select name, age, score from student where age IS NOT NULL

// 判断是null
select name, age, score from student where age IS NULL 
```

### 3. 模糊查询
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

### 3.条件查询 - 逻辑运算
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

### 3.去重
distinct 字段名会去重查询出来的结果
```sql
select distinct class_id, exam_num from student
```

### 3.排序
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

### 3.截断和偏移（分页）LIMIT
```sql
-- LIMIT 后只跟一个整数，表示要截断的数据条数（一次获取几条）
select task_name, due_date from tasks limit 2;

-- LIMIT 后跟 2 个整数，依次表示从第几条数据开始、一次获取几条
select task_name, due_date from tasks limit 2, 2;
```

### 3.条件分支
条件分支 case when 是 SQL 中用于根据条件进行分支处理的语法。它类似于其他编程语言中的 if else 条件判断语句
```sql
SELECT
  name,
  CASE WHEN (name = '鸡哥') THEN '会' ELSE '不会' END AS can_rap
FROM
  student;
```

### 3.时间函数
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

### 3.字符串处理
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

### 3.聚合函数
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

### 3.单字段分组 GROUP BY
```sql
-- 统计学生表中每个班级的平均成绩（avg_score）
select class_id, avg(score) as avg_score from student GROUP BY class_id
-- 可以分组多个字段
select class_id, exam_num, avg(score) as avg_score from student GROUP BY class_id, exam_num
```

### 3.having 子句
HAVING 子句与条件查询 WHERE 子句的区别在于，WHERE 子句用于在 分组之前 进行过滤，而 HAVING 子句用于在 分组之后 进行过滤。
```sql
-- 假设有一个学生表 student，包含以下字段：
-- id（学号）、name（姓名）、class_id（班级编号）、score（成绩）。
-- 请你编写一个 SQL 查询，统计学生表中班级的总成绩超过 
-- 150 分的班级编号（class_id）和总成绩（total_score）。
SELECT class_id, SUM(score) AS total_score 
FROM student GROUP BY class_id HAVING SUM(score) > 150;
```

### 3.关联查询 - cross join
CROSS JOIN 是一种简单的关联查询，不需要任何条件来匹配行，它直接将左表的 每一行 与右表的 每一行 进行组合，返回的结果是两个表的笛卡尔积。
就是左表有3条数据 右表有3条数据 用CROSS JOIN联表查询他们组成9条数据表返回来
```sql
SELECT e.emp_name, e.salary, e.department, d.manager
FROM employees e
CROSS JOIN departments d;
```

