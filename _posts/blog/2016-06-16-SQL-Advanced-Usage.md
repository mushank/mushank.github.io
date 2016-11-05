---

title: "SQL Advanced Usage"
date: 2016-06-16
tag: [SQL]
blog: true
layout: post

---

## TOP 子句

TOP 子句用于规定要返回的记录的数目。

对于拥有数千条记录的大型表来说，TOP 子句是非常有用的。

注释：并非所有的数据库系统都支持 TOP 子句。

### 语法：

```
SELECT TOP number|PERCENT column_name(s)
FROM table_name
```

- 实例

现在，我们希望从上面的 "Persons" 表中选取头两条记录。

我们可以使用下面的 SELECT 语句：

```
SELECT TOP 2 * FROM Persons
```

- SQL TOP PERCENT 实例

现在，我们希望从上面的 "Persons" 表中选取 50% 的记录。

我们可以使用下面的 SELECT 语句：

```
SELECT TOP 50 PERCENT * FROM Persons
```

## MySQL 和 Oracle 中的 SQL SELECT TOP 是等价的

### MySQL 语法

```
SELECT column_name(s)
FROM table_name
LIMIT number
```

- 例子

```
SELECT *
FROM Persons
LIMIT 5
```

### Oracle 语法

```
SELECT column_name(s)
FROM table_name
WHERE ROWNUM <= number
```

- 例子

```
SELECT *
FROM Persons
WHERE ROWNUM <= 5
```

---

## LIKE 操作符

LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。

### SQL LIKE 操作符语法

```
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern
```

- 例子 1

现在，我们希望从上面的 "Persons" 表中选取居住在以 "N" 开始的城市里的人：

我们可以使用下面的 SELECT 语句：

```
SELECT * FROM Persons
WHERE City LIKE 'N%'
```

*提示："%" 可用于定义通配符（模式中缺少的字母）。*

- 例子 4

通过使用 `NOT` 关键字，我们可以从 "Persons" 表中选取居住在*不包含* "lon" 的城市里的人：

我们可以使用下面的 SELECT 语句：

```
SELECT * FROM Persons
WHERE City NOT LIKE '%lon%'
```

---

## SQL 通配符

在搜索数据库中的数据时，SQL 通配符可以替代一个或多个字符。

SQL 通配符必须与 LIKE 运算符一起使用。

在 SQL 中，可使用以下通配符：

| 通配符                      | 描述            |
| ------------------------ | ------------- |
| %                        | 替代一个或多个字符     |
| _                        | 仅替代一个字符       |
| [charlist]               | 字符列中的任何单一字符   |
| [^charlist]或者[!charlist] | 不在字符列中的任何单一字符 |

- 例子 1

现在，我们希望从上面的 "Persons" 表中选取居住的城市*不以* "A" 或 "L" 或 "N" 开头的人：

我们可以使用下面的 SELECT 语句：

```
SELECT * FROM Persons
WHERE City LIKE '[!ALN]%'
```

---

## IN 操作符

IN 操作符允许我们在 WHERE 子句中规定多个值。

### SQL IN 语法

```
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...)
```

- 实例

现在，我们希望从上表中选取姓氏为 Adams 和 Carter 的人：

我们可以使用下面的 SELECT 语句：

```
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
```

---

## BETWEEN 操作符

操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

### SQL BETWEEN 语法

```
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
```

- 实例

如需以字母顺序显示介于 "Adams"（包括）和 "Carter"（不包括）之间的人，请使用下面的 SQL：

```
SELECT * FROM Persons
WHERE LastName
BETWEEN 'Adams' AND 'Carter'
```

> 重要事项：不同的数据库对 BETWEEN...AND 操作符的处理方式是有差异的。某些数据库会列出介于 "Adams" 和 "Carter" 之间的人，但不包括 "Adams" 和 "Carter" ；某些数据库会列出介于 "Adams" 和 "Carter" 之间并包括 "Adams" 和 "Carter" 的人；而另一些数据库会列出介于 "Adams" 和 "Carter" 之间的人，包括 "Adams" ，但不包括 "Carter" 。
>
> 所以，请检查你的数据库是如何处理 BETWEEN....AND 操作符的！

***如需使用上面的例子显示范围之外的人，请使用 NOT 操作符：***

```
SELECT * FROM Persons
WHERE LastName
NOT BETWEEN 'Adams' AND 'Carter'
```



---

未完待续...