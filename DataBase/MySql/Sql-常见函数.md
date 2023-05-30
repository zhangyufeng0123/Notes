# Sql-常见函数

功能：类似于JAVA中的方法，将一组逻辑语句封装在方法体中，对外暴露方法名
好处：1. 隐藏了实现细节，2. 提高代码的重用性
调用：
    `SELECT 函数名() FROM 表;` # 当函数中的参数用到表中的字段时需要加from表

特点：
1. 叫什么（函数名）
2. 干什么（函数功能）

常见函数：
1. 单行函数（字符函数、数学函数、日期函数、其他函数、流程控制函数）如`CONCAT`、`LENGTH`、I`FNULL`等
2. 分组函数
功能：做统计使用，又称为统计函数、聚合函数、组合函数

## 字符函数

### LENGTH 获取参数值的字节个数

```SQL
SELECT LENGTH('--');
```

### CONCAT 拼接字符串

```SQL
SELECT CONCAT(last_name, ' ', first_name) FROM employees;
```

### UPPER、LOWER

```SQL
SELECT UPPER('john');
```

### SUBSTR、SUBSTRING

**案例**：截取从指定索引处后面所有字符

```SQL
SELECT SUBSTR('abcdefg', 3);
```

**案例**：截取从指定索引出指定字符长度的字符

```SQL
SELECT SUBSTR('abcdefg', 1, 3);
```

### INSTR

作用：返回子串第一次出现的索引，如果找不到返回0

```SQL
SELECT INSTR('abcaaa', 'aaa') AS str;
```

### TRIM

作用：去掉字符串前后的指定字符串

```SQL
SELECT LENGTH(TRIM('           abc   '));
```

### LPAD

作用：用指定的字符实现左填充指定长度

```SQL
SELECT LPAD('abc', 10, '8') AS out_put;
```

### RPAD

作用：用指定的字符实现右填充指定长度

```SQL
SELECT RPAD('abc', 10, '8') AS out_put;
```

### REPLACE

```SQL
SELECT REPLACE('abcdefg', 'a', 'c') AS out_put;
```

## 数学函数

### ROUND

作用：四舍五入

```SQL
SELECT ROUND(1.65444, 2);
```

第一个参数是要四舍五入的数字，第二个参数是精确到几位小数，默认是0

### CEIL

作用：向上取整，返回≥该参数的最小整数

```SQL
SELECT CEIL(1.002);
```

### FLOOR

作用：向下取整，返回≤该参数的最大整数

```SQL
SELECT FLOOR(-9.9);
```

### TRUNCATE

作用：截断

```SQL
SELECT TRANCATE(1.69999, 2);
```

第一个参数是你要截断的数字，第二个参数是你要截断的位数
上面的例子返回的是1.69

### MOD

作用：取余

```SQL
SELECT MOD(-19, 3);
```

## 日期函数

### NOW

作用：返回当前系统日期+时间

```SQL
SELECT NOW();
```

### CURDATE

作用：返回当前系统日期，不包含时间

```SQL
SELECT CURDATE();
```

### CURTIME

作用：返回当前时间，不包含日期

```SQL
SELECT CURTIME();
```

### YEAR

作用：可以获取指定的部分，还包括月(MONTH)，日(DAY)，小时(HOUR)，分钟(MINUTE)，秒(SECOND)

```SQL
SELECT YEAR(NOW()) AS 年;
```

### STR_TO_DATE

作用：将日期格式的字符转换成指定的日期格式，通过指定的格式

```SQL
SELECT STR_TO_DAET('9-13-1999', '%m-%d-%Y');
```

### DATE_FORMAT

作用：将日期转换成字符

```SQL
SELECT DATE_FORMAT(NOW(), '%Y年%m月%d日');
```


## 其它函数

```SQL
SELECT VERSION();
SELECT DATABASE();
SELECT USER();
```

## 流程控制函数

### IF函数

作用：if else的效果

```SQL
SELECT IF(10 > 5, 'd', 'x');
```

### CASE

1. 使用方法一：SWITCH CASE的效果
```SQL
// 语法
CASE 要判断的字段或表达式
WHEN 常量1 THEN 要显示的值1或语句1
WHEN 常量2 THEN 要显示的值2或语句2
...
ELSE 要显示的值n或语句n
END
```

**案例**
查询员工的工资，要求
- 部门号=30，显示的工资为1.1倍
- 部门号=40，显示的工资为1.2倍
- 部门号=50，显示的工资为1.3倍
- 其他部门，显示的工资为原始工资

```SQL
SELECT salary AS 原始工资, department_id,
    CASE department_id
    WHEN 30 THEN salary * 1.1
    WHEN 40 THEN salary * 1.2
    WHEN 50 THEN salary * 1.3
    ELSE salary
    END
FROM employees;
```

2. 使用方法二：类似于多重if

```SQL
# 语法
CASE 
WHEN 条件1 THEN 要显示的值1或语句1
WHEN 条件2 THEN 要显示的值2或语句2
...
ELSE 要显示的值n或语句n
END
```

**案例**
查询员工的工资情况
- 如果工资>20000，显示A级别
- 如果工资>15000，显示B级别
- 如果工资>10000，显示C级别
- 否则，显示D级别

```SQL
SELECT last_name, salary, 
    CASE 
    WHEN salary > 20000 THEN 'A'
    WHEN salary > 15000 THEN 'B'
    WHEN salary > 10000 THEN 'C'
    ELSE 'D'
    END AS 级别
FROM employees;
```


## 分组函数

分类：`SUM`求和，`AVG`求平均，`MAX`最大值，`MIN`最小值，`COUNT`计算个数
功能：用作统计使用，又称为聚合函数或统计函数或组函数
特点：
1. `SUM`、`AVG`一般用于处理数值型，`MAX`、`MIN`、`COUNT`可以处理任何类型
2. 是否忽略`NULL`值
3. 可以和`DISTINCT`搭配
4. `COUNT`函数的介绍
5. 和分组函数一同查询的字段要求是`GROUP BY`后的字段


**简单使用**
```SQL
SELECT SUM(salary) FROM employees;
SELECT AVG(salary) FROM employees;
SELECT MAX(salary) FROM employees;
SELECT MIN(salary) FROM employees;
SELECT COUNT(salary) FROM employees;
```
