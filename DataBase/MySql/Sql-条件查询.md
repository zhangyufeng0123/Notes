# Sql-条件查询&排序查询

本篇博客常用数据表

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220437.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220453.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220506.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220522.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220534.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220543.png)

**语法**

```SQL
SELECT 查询列表 FROM 表名 WHERE 筛选条件;
```

分类：
1. 按条件表达式筛选
    条件运算符：> < = != <> >= <= 
2. 按逻辑表达式筛选
    逻辑运算符：&& || ! and or not
3. 模糊查询
    like, between and, in, is null

## 按条件表达式筛选

**案例**：查询工资>12000的员工信息

```SQL
SELECT * FROM employees WHERE salary > 12000;
```

**案例**：查询部门编号不等于90号的员工名和部门编号

```SQL
SELECT last_name, department_id FROM employees WHERE department_id <> 90;
```

## 按逻辑表达式筛选

**案例**：查询工资在10000到20000之间的员工名、工资以及奖金

```SQL
SELECT last_name, salary, ISNULL(commission_pct, 0.0) * salary
FROM employees
WHERE salary >= 10000 and salary <= 20000;
```

**案例**：查询部门编号不是在90到110之间，或者工资高于15000的员工信息

```SQL
SELECT * 
FROM employees
WHERE (department_id < 90 or department_id > 110) or (salary > 15000);
```

## 模糊查询

**通配符**
- %：任意多个字符
- _：任意单个字符

### LIKE

**案例**：查询员工名中包含字符`a`的员工信息

```SQL
SELECT *
FROM employees
WHERE last_name LIKE '%a%';
```

**案例**：查询员工字名中第三个字符为`e`，第五个字符为`a`的员工名和工资

```SQL
SELECT first_name, salary
FROM employees
WHERE first_name LIKE '__e_a%';
```


### BETWEEN AND 

- 使用`between and`可以提高语句的简洁度
- 包含临界值
- 临界值不能调换顺序

**案例**：查询员工编号在100到120之间的员工信息

```SQL
SELECT *
FROM employees
WHERE employee_id BETWEEN 100 AND 120;
```

### in

含义：判断某字段的值是否属于`in`列表中的某一项
特点：
1. 使用`in`提高语句简洁度
2. `in`列表中的值类型必须统一或兼容

**案例**：查询员工的工种编号是IT_PROT, AD_VP, AD_PRES中的一个员工名和工种编号

```SQL
SELECT last_name, job_id
FROM employees
WHERE job_id IN ('IT_PROT', 'AD_VP', 'AD_PRES');
```

### IS NULL 

> = 或 <> 不能用于判断null值
> IS NULL或IS NOT NULL可以判断NULL值

**案例**：查询没有奖金的员工名和奖金率

```SQL
SELECT last_name, commission_pct
FROM employees
WHERE commission_pct IS NOT NULL;
```

> 安全等于 <=>

**案例**：查询没有奖金的员工名和奖金率

```SQL
SELECT last_name, commission_pct
FROM employees
WHERE commission_pct <=> NULL;
```

### IS NULL 和 <=>的对比

`IS NULL`: 仅仅可以判断`NULL`值，建议使用
`<=>`: 既可以判断`NULL`值，也可以判断普通的数值


## 排序查询

语法：
```SQL
SELECT 查询列表
FROM 表
[WHERE 筛选条件]
ORDER BY 排序列表 [ASC|DESC];
```

特点：
    - `ASC`代表升序，`DESC`代表降序，默认升序
    - `ORDER BY`子句中可以支持单个字段、多个字段、表达式、函数、别名
    - `ORDER BY`子句一般是放在查询语句的最后面，`LIMIT`子句除外

**案例**：查询邮箱中包含e的员工信息，并先按邮箱的字节数降序，再按部门号升序

```SQL
SELECT *
FROM employees
WHERE email LIKE '%e%'
ORDER BY LENGTH(email) DESC, department_id ASC;
```
