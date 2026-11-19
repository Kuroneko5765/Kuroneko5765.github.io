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
    - `PRIMARY KEY` 为**主键约束**。一个表格只能有一个主键，并且主键属性唯一。
    - `UNIQUE KEY` 为**唯一约束**。指定了属性必须唯一。
    - `FOREIGN KEY` 为**外键约束**。只能写成表级约束。
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
  - `MODIFY COLUMN <attr> <TYPE>`：将 `<attr>` 属性的**数据类型**修改为 `<TYPE>`。例如 `ALTER TABLE player MODIFY COLUMN name VARCHAR(200);`。
  - `RENAME COLUMN <attr> TO <new>`：将 `<attr>` 属性的**名称**修改为 `<new>`。例如 `ALTER TABLE player RENAME COLUMN hp TO health;`。
  - `ADD COLUMN <attr> <TYPE>`：添加一个属性 `<attr>`，其数据类型为 `<TYPE>`。例如 `ALTER TABLE player ADD COLUMN last_login DATETIME;`。
  - `DROP COLUMN <attr>`：删除属性 `<attr>`。例如 `ALTER TABLE player DROP COLUMN exp;`。
  - `MODIFY <attr> <TYPE> DEFAULT <val>`：为类型为 `<TYPE>` 的属性 `<attr>` 指定默认值 `<val>`。例如 `MODIFY level INT DEFAULT 1;`。
- `DROP TABLE <xxx>;`：删除表 `<xxx>`。
### 操作表中的数据
- `INSERT INTO <xxx> (<attr1>, <attr2>, ...) VALUES (<val11>, <val12>, ...), (<val21>, <val22>, ...), ...;`：向表 `<xxx>` 中对应属性插入元素。
  - `(<attr>, ...)` 也可以不写，此时要按顺序插入所有属性的对应元素.
  - 可以一次插入多行，用逗号隔开。
  - 未指定的值默认为 `NULL`。
  - 例如 `INSERT INTO player (id, name, money, hp) VALUES (1, 'KRNK', 0, 100), (2, 'Herobrine', 100, 200);`。
- `SELECT * FROM <xxx>;`：表示选择 `<xxx>` 表中的所有元素。