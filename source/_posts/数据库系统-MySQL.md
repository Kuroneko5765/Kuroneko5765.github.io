---
title: 数据库系统-MySQL
date: 2026-11-19 16:05:20
categories: Learning
tags: MySQL
---
# MySQL
- **MySQL** 是一个广泛使用的开源关系型数据库管理系统。

## MySQL语法
- 对于关键字，MySQL**不区分大小写**。`SHOW DATABASES;` 和 `show databases;` 拥有一样的效果。下文使用全大写。
  
### 数据库的增删
- `CREATE DATABASE <xxx>;`：创建一个名为 xxx 的数据库。
- `SHOW DATABASES;`：查看当前所有数据库。
- `DROP DATABASE <xxx>;`：删除一个名为 xxx 的数据库。
- `USE <xxx>;`：使用一个数据库。
  
### 操作数据库中的表
- `CREATE TABLE <xxx> ();`：在当前数据库创建一个表格。
  - 括号内填表格的各个属性，以及指定主键、外键等约束。下面给出一个示例。
    ```sql
    CREATE TABLE player (
        id INT NOT NULL PRIMARY KEY, // 列级约束，只能单属性
        name VARCHAR(100) NOT NULL,
        money DECIMAL(10, 2) DEFAULT 0,
        level INT,
        exp INT,
        hp FLOAT,
        birth DATE
        // 主键也可以这样写，为表级约束，可以多属性构成复合主键
        // PRIMARY KEY (id, name)
    );
    ```
  - 该语句创建了一个 `player` 表，主键为 `id`，拥有 `id`、`name`、`money`、`exp`、`hp` 属性。
  - 约束：
    - `NULL` 约束该属性可以为空。
    - `NOT NULL` 约束该属性不能为空。
    - `PRIMARY KEY` 为**主键约束**。一个表格只能有一个主键，并且主键属性唯一。若主键为**复合主键**，只能写成表级约束。
    - `UNIQUE KEY` 为**唯一约束**。指定了属性必须唯一。
    - `FOREIGN KEY` 为**外键约束**，用于保持表之间数据的一致性。一个表的外键必须为另一个表的主键。只能写成表级约束。
    - `DEFAULT <val>` 为默认值。
  - 数据类型：
    - `INT`, `FLOAT`, `DOUBLE` 与常见的使用场景一致。
    - `VARCHAR(<length>)` 是**字符串类型**，最长长度 `<length>`。
    - `DECIMAL(<precision>, <scale>)` 是**精确数字类型**，总位数 `<percision>`，小数位数 `<scale>`。
    - `DATE` 为**日期类型**。格式为 `'YYYY-MM-DD'`。
    - `TIME` 为**时间类型**。格式为 `'HH:MM:SS'`。
    - `DATETIME` 为 `DATE` 和 `TIME` 组合而成的类型。
    
- `DESC <xxx>;`：描述一个表的结构。包括各属性的名称、数据类型、是否为空等。
- `ALTER TABLE <xxx> <...>;`：修改表结构。`<...>` 的常见可用内容：
  - `MODIFY COLUMN <attr> <TYPE>`：将 `<attr>` 属性的**数据类型**修改为 `<TYPE>`。
    - 例如 `ALTER TABLE player MODIFY COLUMN name VARCHAR(200);`。
  - `RENAME COLUMN <attr> TO <new>`：将 `<attr>` 属性的**名称**修改为 `<new>`。
    - 例如 `ALTER TABLE player RENAME COLUMN hp TO health;`。
  - `ADD COLUMN <attr> <TYPE>`：添加一个属性 `<attr>`，其数据类型为 `<TYPE>`。
    - 例如 `ALTER TABLE player ADD COLUMN last_login DATETIME;`。
  - `DROP COLUMN <attr>`：删除属性 `<attr>`。
    - 例如 `ALTER TABLE player DROP COLUMN exp;`。
  - `MODIFY <attr> <TYPE> DEFAULT <val>`：为类型为 `<TYPE>` 的属性 `<attr>` 指定默认值 `<val>`。
    - 例如 `MODIFY level INT DEFAULT 1;`。
- `DROP TABLE <xxx>;`：删除表 `<xxx>`。
  
### 操作表中的数据
- `INSERT INTO <xxx> (<attr1>, <attr2>, ...) VALUES (<val11>, <val12>, ...), (<val21>, <val22>, ...), ...;`：向表 `<xxx>` 中对应属性插入元素。
  - `(<attr>, ...)` 也可以不写，此时要按顺序插入所有属性的对应元素.
  - 可以一次插入多行，用逗号隔开。
  - 未指定的值默认为 `NULL`。
  - 例如 `INSERT INTO player (id, name, money, hp) VALUES (1, 'KRNK', 0, 100), (2, 'Herobrine', 100, 200);`。
- `UPDATE <xxx> SET <attr1> = <val1>, <attr2> = <val2>, ... WHERE <cond>;`：表示将 `<xxx>` 表中符合 `<cond>` 条件的 `<attr>` 属性设置为 `<val>` 值。
  - 例如 `UPDATE player SET hp = 100 WHERE name = 'KRNK';`。
- `DELETE FROM <xxx> WHERE <cond>;`：删除 `<xxx>` 表中符合 `<cond>` 条件的行。
  
### SELECT 查询语句
`SELECT` 语句具有繁多的功能，见下。
```sql
SELECT <attr1>, <attr2>, ...
FROM <xxx>
WHERE <cond1>
GROUP BY <attr_group>
HAVING <cond2>
ORDER BY <attr_order> [ASC|DESC]
LIMIT <offset>, <count>;
```
- 以上的子句都会按顺序执行。
- `SELECT (DISTINCT) <attr>`：表示选择哪些属性。属性可以通过 `DISTINCT` 去重。特别地，`SELECT *` 表示选择全部。`<attr>` 也可改成**聚合函数**，见下。聚合函数会经过 `WHERE` 条件子句的筛选。
  - `COUNT(*)`：返回该表的行数。
  - `AVG(<attr>)`：返回 `<attr>` 的**平均值**。
  - `SUM(<attr>)`：返回 `<attr>` 的**总和**。
  - `MAX(<attr>)`：返回 `<attr>` 的**最大值**。
  - `MIN(<attr>)`：返回 `<attr>` 的**最小值**。
  - `ROUND(<attr>)`：返回 `<attr>` **四舍五入**的结果。
  - `SUBSTR(<str>, <idx>, <len>)`：**截取**字符串 `<str>`，从 `<idx>` 开始，截取 `<len>` 长度。
- `FROM <xxx>`：表示从 `<xxx>` 表选择。
- `WHERE <cond1>`：对行以 `<cond1>` 条件进行过滤。**不能**使用聚合函数。
- `GROUP BY <attr_group>`：表示根据 `<attr_group>` 属性来分组。该属性相同的行会被分到同一组，其他属性**必须**通过聚合函数来取值。
  - 例如 `SELECT level, COUNT(*) FROM player GROUP BY level;` 可以显示出每个存在的等级的人数分别有多少。
- `HAVING <cond2>`：对已分好的组以 `<cond2>` 进行条件过滤。**可以**使用聚合函数。
  - 例如 `SELECT level, COUNT(*) FROM player GROUP BY level HAVING COUNT(*) > 5;` 可以显示出每个存在的等级的多于 5 个的人数分别有多少。
- `ORDER BY <attr_order> [ASC|DESC]`：以 `<attr_order>` 属性对行进行排序。`ASC` 表示升序，`DESC` 表示降序。默认为 `ASC`。
  - 也可以多个 `<attr_order>` 一起构成多重排序，之间用逗号隔开。例如 `ORDER BY level ASC, money DESC`。
- `LIMIT <offset>, <count>`：从第 `<offset> + 1` 行开始，最多显示 `<count>` 行。`<offset>` 可以省略，则默认从第一行开始。
  - 例如 `LIMIT 2, 3` 表示从第 3 行开始显示 3 行，也就是第 3 ~ 5 行。

### WHERE 条件语句
- 不同于常见编程语言，SQL 使用 `=` 代表等于，`<>` 代表不等于，`IS (NOT) NULL` 判定是否为空。
- 使用 `AND`, `OR`, `NOT` 来连接多个子条件。
- `IN (<val1>, <val2>, ...)`：筛选某属性值落在指定集合中的行。
  - 例如 `WHERE level IN (1, 3, 5)` 可以筛选等级为 1 或 3 或 5 的行。
- `BETWEEN <val1> AND <val2>` 用于筛选某属性值落在指定区间内的行。
  - 例如 `WHERE money BETWEEN 0 AND 100` 可以筛选金钱从 0 到 100 的行。
- `LIKE '<pattern>'`：**模糊匹配**字符串，可视为简易版的正则匹配。可用符号包括 `%` 与 `_`。`%` 代表**任意长度子字符串**，`_` 代表**任意字符**。
  - 例如 `%th` 可以匹配 `Smith` 与 `Ith`，但 `_th` 只能匹配 `Ith`。
  - 例如 `WHERE name LIKE 'K%'` 可以筛选名称首字符为 `K` 的行。
- `REGEXP '<pattern>'`：**正则匹配**字符串。在此不再赘述正则匹配。

### 其他常用语句
- `UNION (ALL)`：对多个查询结果求**并集**。若加上 `ALL`，则求得的结果集不去重。
  - 例如 `SELECT * FROM player WHERE money > 100 UNION SELECT * FROM player WHERE level > 5;`。
- `INTERSECT`：对多个查询结果求**交集**。
- `EXCEPT`：对多个查询结果求**差集**。
- `<attr> AS <alias>`：对属性起一个别名。可以用在后续查询中，以及查询结果的显示中。
- 