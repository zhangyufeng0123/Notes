# Sql-查询


## 基础查询

**语法**

```SQL
SELECT 查询列表 FROM 表名;
```

**特点**

1. 查询列表可以是：表中的字段、常量值、表达式、函数
2. 查询的结果是一个虚拟的表格

以下的表是本篇博客中常用的表

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220437.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220453.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220506.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220522.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220534.png)

![Img](https://cdn.jsdelivr.net/gh/zhangyufeng0123/ImageHosting/img/yank-note-picgo-img-20230529220543.png)


**示例**

查询表中的单个字段

```SQL
SELECT last_name FROM employees;`
```

查询表中的多个字段

```SQL
SELECT last_name, salary, email
FROM employees;
```

查询表中的所有字段

```SQL
SELECT *
FROM employees;
```

查询常量值（一般是用来检验数据库是否连接成功）

```SQL
SELECT 1;
```

查询表达式

```SQL
SELECT 100 * 98;
```

查询函数

```SQL
SELECT VERSION();
```

## 查询中用到的方法

### 起别名

好处：
    1. 便于理解
    2. 如果要查询的字段有重名的情况，使用别名可以区分开来

方法一：使用AS

```SQL
SELECT 100 % 98 AS result;
```

方法二：使用空格

```SQL
SELECT last_name 姓, first_name 名
FROM employees;
```

注意的是，起的别名不能是关键词，如果遇到了别名是关键词的话要加上着重符``

```SQL
# SELECT salary AS out put FROM employees;
SELECT salary AS `out put` FROM employees;
```

### 去重

案例：查询员工表涉及到的所有的部门编号

```SQL
SELECT DISTINCT department_id FROM employees;
```

如果没有用`DISTINCT`，那么返回的结果中有重复的部门编号

### +号的作用

仅仅只有一个功能：运算符

1. `SELECT 100 + 90;` 两个操作符都为数值型，则做加法运算
2. `SELECT '123' + 90;` 其中一方为字符型，试图将字符型数值转换成数值型，如果转换成功，则继续做加法运算
3. `SELECT 'john' + 90;` 如果转换失败，则将字符型数值转换为`0`
4. `SELECT NULL + 10;` 只要其中一方为`NULL`，则结果为`NULL`