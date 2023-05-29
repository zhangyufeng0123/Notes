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
