# 了解SQL

## 数据库基础

### 数据库

保存有组织的数据的容器

数据库软件称为数据库管理系统（DBMS）

### 表

某种特定类型数据的结构化清单

> schema 模式
>
> 关于数据库和表的布局及特性的信息

### 列和数据类型

列为表中的一个字段，表由一个或多个列组成

> 每个表列都有相应的数据类型，它限制（或允许）该列中存储的数据

### 行

表中的一个记录。

### 主键

表中每一行都应该有一列（或几列）可以唯一标识自己。

- 任意两行都不具有相同的主键值；
- 每一行都必须具有一个主键值（主键列不允许空值 NULL）；
- 主键列中的值不允许修改或更新；
- 主键值不能重用（如果某行从表中删除，它的主键不能赋给以后的新行）。



# 检索数据

## SELECT

### 检索单个列

```mysql
SELECT prod_name FROM Products;
```

### 检索多个列

```mysql
SELECT prod_id, prod_name, prod_price FROM Products;
```

### 检索所有列

```mysql
SELECT * FROM Products;
```

### 检索不同的值

```mysql
SELECT DISTINCT vend_id FROM Products;
```

DISTINCT 关键字作用于所有的列，不仅仅是跟在其后的那一列。

如果使用 DISTINCT 关键字，它必须直接放在列名的前面。

### 限制结果

```mysql
SELECT prod_name
FROM Products
LIMIT 5;
```

LIMIT 5指示MySQL等 DBMS 返回不超过 5 行的数据。

```mysql
SELECT prod_name
FROM Products
LIMIT 5 OFFSET 5;
```

LIMIT 5 OFFSET 5 指示 MySQL 等 DBMS 返回从第 5 行起的 5 行数据。

> MySQL、MariaDB和 SQLite可以把 LIMIT 4 OFFSET 3 语句简化为 LIMIT 3,4。

### 注释

```mysql
#这是一条注释
/* 这是一条
很长的注释 */
-- 这是一条注释
```



# 排序检索数据

## 排序数据

```mysql
SELECT prod_name
FROM Products
ORDER BY prod_name;
```

> **ORDER BY** 子句的位置 
>
> 在指定一条 ORDER BY 子句时，应该保证它是 SELECT 语句中最后一条子句。如果它不是最后的子句，将会出错。

> 用非检索的列排序数据是完全合法的。

## 按多个列排序

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price, prod_name;
```

在按多个列排序时，排序的顺序完全按规定进行。即先按照prod_price排序，如果有相同的再按照prod_name排序

## 按列位置排序

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY 2, 3;
```

## 指定排序方向

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
ORDER BY prod_price DESC; -- 降序
```

DESC关键字只应用到直接位于其前面的列名。



# 过滤数据

## WHERE 子句

```mysql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price = 3.49;
```

数据过滤应在SQL中进行而不是应用层进行

## WHERE 子句操作符

| 操作符  |        说明        |
| :-----: | :----------------: |
|    =    |        等于        |
|   <>    |       不等于       |
|   !=    |       不等于       |
|    <    |        小于        |
|   <=    |      小于等于      |
|   !<    |       不小于       |
|    >    |        大于        |
|   >=    |      大于等于      |
|   !>    |       不大于       |
| BETWEEN | 在指定的两个值之间 |
| IS NULL |       为NULL       |



### 检查单个值

```mysql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price < 10;
```

### 不匹配检查

```mysql
SELECT vend_id, prod_name
FROM Products
WHERE vend_id <> 'DLL01';
```

> 何时使用引号 
>
> varchar或datetime

### 范围值检查

```mysql
SELECT prod_name, prod_price
FROM Products
WHERE prod_price BETWEEN 5 AND 10;
```

### 空值检查

```mysql
SELECT prod_name
FROM Products
WHERE prod_price IS NULL;
```



# 高级数据过滤

## 组合 WHERE 子句

### AND操作符

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' AND prod_price <= 4;
```

### OR操作符

```mysql
SELECT prod_id, prod_price, prod_name
FROM Products
WHERE vend_id = 'DLL01' OR vend_id = 'BRS01';
```

### 求值顺序

AND 在求值过程中优先级更高



## IN 操作符

IN 取一组由逗号分隔、括在圆括号中的合法值。

```mysql
SELECT prod_name, prod_price
FROM Products
WHERE vend_id IN ('DLL01','BRS01')
ORDER BY prod_name;
```



IN的优点

> - 在有很多合法选项时，IN 操作符的语法更清楚，更直观。
> - 在与其他 AND 和 OR 操作符组合使用 IN 时，求值顺序更容易管理。
> - IN 操作符一般比一组 OR 操作符执行得更快
> - IN 的最大优点是可以包含其他 SELECT 语句，能够更动态地建立WHERE 子句。





## NOT 操作符

```mysql
SELECT prod_name
FROM Products
WHERE NOT vend_id = 'DLL01'
```

为什么使用 NOT？对于这里的这种简单的 WHERE 子句，使用 NOT 确实没有什么优势。

但在更复杂的子句中，NOT 是非常有用的。例如，在与 IN 操作符联合使用时，NOT 可以非常简单地找出与条件列表不匹配的行。



# 用通配符进行过滤

## LIKE 操作符

### 百分号（%）通配符

```mysql
SELECT prod_id, prod_name 
FROM Products 
WHERE prod_name LIKE 'Fish%';
```

### 下划线（_）通配符

MySQL不支持

### 方括号（[ ]）通配符

MySQL不支持



# 创建计算字段

## 计算字段

存储在表中的数据都不是应用程序所需要的。我们需要直接从数据库中检索出转换、计算或格式化过的数据，而不是检索出数据，然后再在客户端应用程序中重新格式化。



## 拼接字段

```mysql
SELECT Concat(vend_name, ' (', vend_country, ')') 
FROM Vendors
ORDER BY vend_name;
```

> TRIM 函数 
>
> RTRIM()（去掉字符串右边的空格）
>
> LTRIM()（去掉字符串左边的空格）
>
> TRIM()（去掉字符串左右两边的空格）



## 执行算术计算

```mysql
SELECT prod_id,
 quantity,
 item_price,
 quantity*item_price AS expanded_price
FROM OrderItems
WHERE order_num = 20008;
```



SQL算术操作符

| 操作符 | 说明 |
| :----: | :--: |
|   +    |  加  |
|   -    |  减  |
|   *    |  乘  |
|   /    |  除  |



# 使用函数处理数据



- 用于处理文本字符串（如删除或填充值，转换值为大写或小写）的文本函数。
- 用于在数值数据上进行算术操作（如返回绝对值，进行代数运算）的数值函数。
- 用于处理日期和时间值并从这些值中提取特定成分（如返回两个日期之差，检查日期有效性）的日期和时间函数。
- 用于生成美观好懂的输出内容的格式化函数（如用语言形式表达出日期，用货币符号和千分位表示金额）。
- 返回 DBMS 正使用的特殊信息（如返回用户登录信息）的系统函数



## 文本处理函数

常用的文本处理函数

|   函数    |         说明          |
| :-------: | :-------------------: |
|  LEFT()   | 返回字符串左边的字符  |
| LENGTH()  |   返回字符串的长度    |
|  LOWER()  |  将字符串转换为小写   |
|  LTRIM()  | 去掉字符串左边的空格  |
|  RIGHT()  | 返回字符串右边的字符  |
|  RTRIM()  | 去掉字符串右边的空格  |
| SUBSTR()  | 提取字符串的组成部分  |
| SOUNDEX() | 返回字符串的SOUNDEX值 |
|  UPPER()  |  将字符串转换为大写   |



SOUNDEX()函数的例子

```mysql
SELECT cust_name, cust_contact
FROM Customers
WHERE SOUNDEX(cust_contact) = SOUNDEX('Michael Green');
```























