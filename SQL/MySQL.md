# MySQL

Re1ife for MySQL



## 查询数据

### 基本查询

```sql
SELECT * FROM <表名>
```



查询student表中所有数据

```sql
SELECT * FROM student;
```

`SELECT`并不要求一定有`FROM`，可以通过以下语句测试数据库连接

```sql
SELECT 1;
```



### 条件查询

```sql
SELECT * FROM <表名> WHERE <条件表达式>
```



查询分数在80分以上的学生记录：

```sql
SELECT * FROM students WHERE score >= 80;
```

条件表达式可以用`<条件1> AND <条件2>`表达满足条件1并且满足条件2

```sql
SELECT * FROM students WHERE score >= 80 AND gender = 'M';
```

第二种条件是`<条件1> OR <条件2>`

```sql
SELECT * FROM students WHERE score >= 80 OR gender = 'M';
```

第三种条件是`NOT <条件>`

```sql
SELECT * FROM students WHERE NOT class_id = 2;
```

等价于

```sql
SELECT * FROM students WHERE class_id <> 2;
```



如果不加括号，条件运算按照`NOT`、`AND`、`OR`的优先级进行，即`NOT`优先级最高，其次是`AND`，最后是`OR`。加上括号可以改变优先级。



### 投影查询

让结果集仅包含指定列，称为投影查询

```sql
SELECT id, score, name FROM students;
```



### 排序

按score从低到高

```sql
SELECT id, name, gender, score FROM students ORDER BY score;
```

从高到低

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC;
```

先按`score`列倒序，如果有相同分数的，再按`gender`列排序：

```sql
SELECT id, name, gender, score FROM students ORDER BY score DESC, gender;
```



### 分页查询

把结果集分页，每页3条记录。要获取第1页的记录，可以使用`LIMIT 3 OFFSET 0`：

`OFFSET`后跟跳过的记录条数

```sql
SELECT id, name, gender, score
FROM students
ORDER BY score DESC
LIMIT 3 OFFSET 0;
```



- `LIMIT`总是设定为`pageSize`；
- `OFFSET`计算公式为`pageSize * (pageIndex - 1)`。



在MySQL中，`LIMIT 15 OFFSET 30`还可以简写成`LIMIT 30, 15`。

使用`LIMIT <M> OFFSET <N>`分页时，随着`N`越来越大，查询效率也会越来越低。



### 聚合查询

`COUNT(*)`表示查询所有列的行数，

```sql
SELECT COUNT(*) FROM students;
```



#### 分组

如果我们要统计一班的学生数量，我们知道，可以用`SELECT COUNT(*) num FROM students WHERE class_id = 1;`。

但如果要继续统计二班、三班的学生数量，难道必须不断修改`WHERE`条件来执行`SELECT`语句吗？

No

SQL还提供了“分组聚合”的功能。

```sql
SELECT class_id, COUNT(*) num FROM students GROUP BY class_id;
```





