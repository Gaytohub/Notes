# PostgreSQL

## MacOS 

启动PostgreSQL服务：brew services start postgresql@9.4 

postgres用户连接postgres数据库：psql -U postgres -d postgres 

停止PostgreSQL服务：brew services stop postgresql@9.4 



## 数据类型

帮助我们在建表时为每个字段设置需要的数据类型。还可以使用 CREATE TYPE 命令在数据库中创建新的数据类型。

### 数值类型

| 名字             | 存储长度 | 描述                 | 范围                       |
| ---------------- | -------- | -------------------- | -------------------------- |
| smallint         | 2字节    | 小范围整数           | -32768到+32767             |
| integer          | 4字节    | 常用的整数           | -2147483648 到 +2147483647 |
| bigint           | 8字节    | 大范围整数           | ...                        |
| decimal          | 可变长   | 用户指定的精度，精确 |                            |
| numeric          | 可变长   | 用户指定的精度，精确 |                            |
| real             | 4字节    | 可变精度，不准确     | 6位十进制数字精度          |
| double precision | 8字节    | 可变精度，不准确     | 15位十进制数字精度         |
| smallserial      | 2字节    | 自增的小范围整数     | 1到32767                   |
| serial           | 4字节    | 自增整数             | 1到2147483647              |
| bigserial        | 8字节    | 自增的大范围整数     | 1到...                     |

### 货币类型

money类型存储带有固定小数精度的货币金额。**因为存在舍入错误的可能性，所以不建议使用浮点数处理货币类型。**

| 名字  | 存储长度 | 描述     |
| ----- | -------- | -------- |
| money | 8字节    | 货币金额 |

### 字符类型

| 名字       | 描述             |
| ---------- | ---------------- |
| varchar(n) | 变长，有长度限制 |
| char(n)    | 定长，不足补空白 |
| Text       | 变长，无长度限制 |

### 日期和时间类型

| 名字      | 存储长度                          | 描述                                                         |
| --------- | --------------------------------- | ------------------------------------------------------------ |
| timestamp | 8字节                             | 能够表示日期和时间，有带时区和不带时区两种。精确度为一毫秒。 |
| date      | 4字节                             | 只用于记录日期。精确度为一天。                               |
| time      | 8字节（不带时区）12字节（带时区） | 只用于一日之内的时间，从 00:00:00+1459 到 24:00:00-1459，精确度为一毫秒。 |
| interval  | 12字节                            | 表示时间间隔。精确度为一毫秒。                               |

### 布尔类型

| 名称    | 存储长度 | 描述         |
| ------- | -------- | ------------ |
| boolean | 1字节    | True / False |



## 创建数据库

PostgreSQL创建数据库可以用下面两种方式：

1. 使用 **CREATE DATABASE** SQL语句来创建。
2. 使用 **createdb** 命令来创建。



第一种方式：

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508152530015.png" alt="image-20210508152530015" style="zoom:50%;" />	

第二种方式：

```
createdb -U postgres newTest
```

  该命令会通过 postgres 用户创建 newTest 数据库



## 选择数据库

使用 **\l** 可以查看已经存在的数据库。

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508153102802.png" alt="image-20210508153102802" style="zoom:50%;" />	

使用 **\c + 数据库名称** 可以选择数据库

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508153500837.png" alt="image-20210508153500837" style="zoom:50%;" />	

## 删除数据库

PostgreSQL删除数据库可以用下面两种方式：

1. 使用 **DROP DATABASE** SQL语句来创建。
2. 使用 **dropdb** 命令来创建。



第一种方式：

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508154015814.png" alt="image-20210508154015814" style="zoom:50%;" />	

第二种方式：

```
dropdb -U postgres newTest
```

该命令会通过 postgres 用户删除 newTest 数据库



## 创建表格

PostgreSQL 使用 CREATE TABLE 语句创建数据库表格。

```
CREATE TABLE table_name(
	column1 dataType,
	column1 dataType,
	column3 dataType,
	...........,
	columnN dataType,
	PRIMARY KEY( 一个或多个列 )
);
```

比如以下实例，创建了名为 COMPANY 的表格。

```mysql
CREATE TABLE COMPANY(
	ID INT PRIMARY KEY NOT NULL,
  NAME TEXT NOT NULL,
  AGE INT NOT NULL,
  ADDRESS CHAR(50),
  SALARY REAL
);
```

通过 **\d**命令可以查看表格创建是否成功：

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508162148295.png" alt="image-20210508162148295" style="zoom:50%;" />	

使用 **\d tablename** 查看表格信息：

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508162251940.png" alt="image-20210508162251940" style="zoom:50%;" />	



## 删除表格

PostgreSQL 使用 DROP TABLE 语句删除表格，会连同表格数据、规则、触发器等一同删除。

使用方法：

```
DROP TABLE table_name
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508162958296.png" alt="image-20210508162958296" style="zoom:50%;" />	



## PostgreSQL 模式（SCHEMA）

PostgreSQL 模式（SCHEMA）可以看作是一个表的集合。相同的表名称可以用于不同的模式中而不会出现冲突。比如：schema1 和 myshcema 都可以包含名为 mytable 的表格。

使用模式的优势：

- 允许多个用户使用一个数据库且不会发生干扰。
- 将多个表格通过逻辑的形式组织在一起，便于管理。



创建模式，使用方法：

```
CREATE SCHEMA schema_name;
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508164724970.png" alt="image-20210508164724970" style="zoom:50%;" />	

终端输出 CREATE SCHEMA 表示模式创建成功。

接下来再该模式下创建表格，使用方法如下：

```
CREATE TABLE schema_name.table_name(
	.....
);
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508165134169.png" alt="image-20210508165134169" style="zoom:50%;" />	

执行查询，查看表格是否成功创建：

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508165224779.png" alt="image-20210508165224779" style="zoom:50%;" />	



删除模式，使用方法：

删除一个空的模式（其中的表格完全删除）：

```
DROP SCHEMA schema_name;
```

删除一个模式以及该模式下的所有对象：

```
DROP SCHEMA schema_name CASCADE;
```

<img src="/Users/paranoid/Library/Application Support/typora-user-images/image-20210508165752692.png" alt="image-20210508165752692" style="zoom:50%;" />	



## 插入语句

PostgreSQL 使用 INSERT INTO 进行数据插入，其语法格式如下：

```mysql
INSERT INTO table_name (column1, column2, column3, ... columnN) VALUES (value1, value2, value3, ... columnN);
```

**NOTICE：**使用INSERT INTO 进行数据插入的时候，字段列数必须和数据值数量相同，并且顺序要对应。如果是向表中的所有字段插入数据，可以不指定字段，只指定插入的值：

```mysql
INSERT INTO table_name VALUES (value1, value2, value3, ... columnN);
```

以上是插入单行数据的使用方法，当然也可以一次性插入多行数据，使用方法如下：

```mysql
INSERT INTO table_name (column1, column2, column3, ... columnN) VALUES (value1_1, value1_2, value1_3, ... column1_N), (value2_1, value2_2, value2_3, ... column2_N), (value3_1, value3_2, value3_3, ... column3_N);
```



## 查询语句

PostgreSQL 使用SELECT 语句从数据库中选取数据。结果会存储在一个结果表中，称为**结果集**。

使用方法如下：

```mysql
SELECT column1, column2, ... columnN FROM table_name;
```

如果想读取表中全部数据可以使用：

```mysql
SELECT * FROM table_name;
```



## 运算符

运算符是一些保留关键字或字符，用在 WHERE 子句中，作为过滤条件。

### 算数运算符

假设 a = 3, b = 2 , 则有如下表格：

| 运算符 |     描述     |       实例        |
| :----: | :----------: | :---------------: |
|   +    |      加      |  a + b 结果为 5   |
|   -    |      减      |  a - b 结果为 1   |
|   *    |      乘      |  a * b 结果为 6   |
|   /    |      除      |  a / b 结果为 1   |
|   %    |     取余     |  a % b 结果为 1   |
|   ^    |     指数     |  a ^ b 结果为 9   |
|  \|/   |    平方根    |  \|/ 25 结果为 5  |
| \|\|/  |    立方根    | \|\|/ 27 结果为 3 |
|   !    |     阶乘     |   5！结果为 120   |
|   !!   | 阶乘（前缀） |  !!5 结果为 120   |

### 比较运算符

常见的比如， =, !=, <> (不等于), >, <, >=, <= .

### 逻辑运算符

| 运算符 |                             描述                             |
| :----: | :----------------------------------------------------------: |
|  AND   |            逻辑与。两个操作数都为真，则条件为真。            |
|  NOT   | 逻辑非。逆转当前操作数都状态。PostgreSQL 有 NOT EXISTS, NOT BETWEEN, NOT IN 操作符。 |
|   OR   |        逻辑或。两个操作数中任意一个为真，则条件为真。        |

### 位运算符

PostgreSQL 支持位运算符。比如 ： & （按位与） ｜ （按位或）   # （异或）～（取反）<< （二进制左移）>>（二进制右移）



## 表达式

表达式类似一个公式，将其运用在查询语句中，用来查找数据库中指定条件的结果集。

有布尔表达式，数字表达式，日期表达式。

| 类别       | 事例                                                     |
| ---------- | -------------------------------------------------------- |
| 布尔表达式 | field_name = VALUE, field_name > VALUE...                |
| 数字表达式 | 数学表达式（+,-,*,/) 还有内置的数学函数，如avg,sum,count |
| 日期表达式 | SELECT  CURRENT_TIMESTAMP (返回系统的日期和时间)         |



## WHERE 子句

当需要从单张表或者多张表中查询数据的时候，就可以在SELECT 子句中添加 WHERE 子句，这样可以过滤我们不需要的数据。

WHERE 子句不仅可以用于 SELECT 语句，同时可以用于 UPDATE 、 DELETE 等语句中。

在 WHERE 子句中可以使用不同的运算符构成表达式，来筛选我们需要的内容。

首先使用如下 SQL 语句创建 COMPANY 表：

```mysql
DROP TABLE COMPANY;
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);
INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Paul', 32, 'California', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Allen', 25, 'Texas', 15000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Teddy', 23, 'Norway', 20000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (4, 'Mark', 25, 'Rich-Mond ', 65000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (5, 'David', 27, 'Texas', 85000.00 );

INSERT INTO COMPANY (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Kim', 22, 'South-Hall', 45000.00 );

INSERT INTO COMPANY VALUES (7, 'James', 24, 'Houston', 10000.00 );
```

以下是一些 WHERE 子句的实例：

```mysql
// 查找年龄大于 25 且 薪水大于 65000 的人员信息
SELECT * FROM COMPANY WHERE AGE >= 25 AND SALARY >= 65000;

// 查找年龄大于 25 或者 薪水大于 65000 的人员信息
// SELECT * FROM COMPANY WHERE AGE >= 25 OR SALARY >= 65000;

// 查找年龄字段不为空的人员信息
SELECT * FROM COMPANY WHERE AGE IS NOT NULL;

// 查找 NAME 字段中以 Pa 开头的人员信息
SELECT * FROM COMPANY WHERE NAME LIKE 'Pa';

// 查找年龄字段为 25 或 27 的数据
SELECT * FROM COMPANY WHERE AGE IN (25, 27);

// 查找年龄字段不为 25 或 27 的数据
SELECT * FROM COMPANY WHERE AGE NOT IN (25, 27);

// 查找年龄字段在 25 到 27 的数据
SELECT * FROM COMPANY WHERE AGE BETWEEN 25 AND 27;
```

以下是子查询的例子：

```mysql
// 子查询语句读取 薪水大于 65000 的人员年龄数据，通过 EXISTS 运算符判断是否有返回行，如果有返回行则读取所有的年龄字段。
SELECT AGE FROM COMPANY WHERE EXISTS (SELECT AGE FROM COMPANY WHERE SALARY > 65000);

// 自查询语句读取 薪水大于 65000 的人员年龄数据，再通过 > 运算符查询大于该年龄字段数据。
SELECT * FROM COMPANY WHERE AGE > (SELECT AGE FROM COMPANY WHERE SALARY > 65000);
```



## UPDATE 语句

数据库的更新操作，需要由 UPDATE 语句来完成。

UPDATE 修改数据的 SQL 语法 ：

```mysql
UPDATE table_name
SET column1 = value1, column2 = value2 ..., columnN = valueN
WHERE [CONDITIONS];
```



## DELETE 语句

数据库的删除操作，需要由 DELETE 语句来完成。

```mysql
DELETE FROM table_name
WHERE [CONDITIONS];
```

如果没有 WHERE 子句，表中的所有记录都将会被删除。 有 WHERE 子句则删除符合 WHERE 子句条件的行。



## LIKE 子句

在需要获取某些字符的数据时，可以使用 LIkE 子句。 LIKE 子句中，通常会和通配符搭配使用，通配符可以表示任意字符，主要有两种：**百分号 %  下划线 _ **  如果没有使用这两种通配符，那么 LIKE 子句和使用 = 得到的结果是一样的。

以下是 LIKE 子句的例子：

```mysql
## 因为 SALARY 字段是整形数据，而 LIKE 子句只能对字符进行比较，所以 需要使用 ::text 转换为字符串类型

// 找出 SALARY 字段中以 200 开头的数据
WHERE SALARY::text LIKE '200%'

// 找出 SALARY 字段中含有 200 字符的数据
WHERE SALARY::text LIKE '%200%'

// 找出 SALARY 字段中第二三个位置上都是 0 的数据
WHERE SALARY::text LIKE '_00%'

// 找出 SALARY 字段中以 2 开头的字符长度大于 3 的数据
WHERE SALARY::text LIKE '2 % %'

// 找出 SALARY 字段中以 2 结尾的数据
WHERE SALARY::text LIKE '%2'

// 找出 SALARY 字段中第二位是 2 且以 3 结尾的数据
WHERE SALARY::text LIKE '_2%3'

// 找出 SALARY 字段中以 2 开头 3 结尾 并且长度为 5 的数据
WHERE SALARY::text LIKE '2___3'
```



## LIMIT 子句

LIMIT 子句主要是用于限制 SELECT 语句中查询到的数据的数量。

LIMIT 子句的使用方法：

```mysql
SELECT column1, column2, columnN
FROM table_name 
LIMIT [num of rows]

// 可以和 offset 搭配
SELECT column1, column2, columnN
FROM table_name 
LIMIT [num of rows] OFFSET [row num]

// 读取 COMPANY 表中前四条数据
SELECT * FROM COMPANY LIMIT 4

// 从第三行开始读取 COMPANY 表中三条数据
SELECT * FROM COMPANY LIMIT 3 OFFSET 2
```



## ORDER BY 子句

使用 ORDER BY 可以根据一列或者多列数据进行升序或者是降序排列。

ORDER BY 子句的使用方法：

```MYSQL
SELECT column-list
FROM table_name
WHERE [conditions]
ORDER BY column1, column2 ... columnN [ASC | DESC]

// 查询所有数据，并根据姓名薪水进行升序排序
SELECT * FROM COMPANY ORDER BY NAME, SALARY ASC

// 查询所有数据，并根据姓名升序，薪水降序
SELECT * FROM COMPANY ORDER BY NAME ASC, SALARY DESC
```



## GROUP BY 子句

GROUP BY 子句和 SELECT 语句一起使用，用来对相同的数据进行分组。 GROUP BY 子句必须在 WHERE 子句之后，ORDER BY 子句之前。GROUP BY 子句中，可以对一列或者是多列进行分组，但是被分组的列必须位于列清单中。

GROUP BY 子句的使用方法：

```mysql
SELECT column-list
FROM table_name
WHERE [conditions]
GROUP BY column1, column2... columnN
ORDER BY column1, column2... columnN

// 根据 NAME 字段进行分组，找出每个人的工资总额
SELECT NAME, SUM(SALARY) FROM COMPANY GROUP BY NAME
```



## WITH 语句

...





## HAVING 子句

HAVING 子句可以让我们筛选分组后的各组数据。WHERE 子句在所选择的列上设置条件，而 HAVING 子句则在由 GROUP BY 子句创建的分组上设置条件。

HAVING 子句必须放置在GROUP BY 子句后面，ORDER BY 子句前面，下面是 HAVING 子句的使用方法：

```mysql
SELECT column1, column2
FROM table1, table2
WHERE [conditions]
GROUP BY column1, column2
HAVING [conditions]
ORDER BY column1, column2

// 数据根据 NAME 字段值进行分组，查出名称计数大于1的数据
SELECT NAME FROM COMPANY GROUP BY NAME HAVING COUNT(NAME) > 1
```



## DISTINCT 关键字

DISTINCT 关键字和 SELECT 语句一起使用，用于去除重复记录，只获取唯一记录。

DISTINCT 关键字使用方法如下：

```mysql
SELECT DISTINCT column1, column2 ... columnN
FROM table_name
WHERE [conditions]

// 获取 COMPANY 表中去重后的名字记录
SELECT DISTINCT NAME FROM COMPANY
```



## 约束

使用约束可以规定表中的数据规则。如果存在违反约束的行为，行为就会被终止。

添加约束有两种方式，一种是在创建表时规定（通过 CREATE TABLE 语句），另外一种是在表创建之后通过 ALTER TABLE 语句指定。约束能够确保数据库中数据的准确性和可靠性。

约束可以是列级的，也可以是表级的。列级约束仅适用于列， 表级约束会被应用到整个表。

以下是常用约束：

|   关键字    |                             意义                             |
| :---------: | :----------------------------------------------------------: |
|  NOT NULL   |                        该字段不能为空                        |
|   UNIQUE    |                       该字段值不可重复                       |
| PRIMARY KEY | NOT NULL 和 UNIQUE 的结合。确保某列（或两个及多个列的结合）有唯一标识，有助于更快速地找到表中的一个特定记录 |
| FOREIGN KEY | 外键约束指定一个列（或一组列）中的值必须匹配出现在另一个表中的某些行的值。这两个关联表之间具有引用完整性。 |
|    CHECK    |                  保证列中的值符合指定的条件                  |
|  EXCLUSION  | **排他约束保证如果将任何两行的指定列或表达式使用指定操作符进行比较，至少其中一个操作符比较将会返回否或空值** （并不理解是什么意思） |



### FOREIGN KEY 约束

通常一个表中的FOREIGN KEY 指向另一个表中得到 UNIQUE KEY ，即维护了两个相关表之间的引用完整性。

使用方法如下：

```mysql
// 创建 COMPANY 表，并添加 5 个字段
CREATE TABLE COMPANY(
	ID INT PRIMARY KEY,
	NAME TEXT NOT NULL,
	AGE INT NOT NULL,
	ADDRESS CHAR(50),
	SALARY REAL
);

// 创建 DEPARTMENT 表，添加 3 个字段， EMP_ID 就是外键，参照 COMPANY 的ID
CREATE TABLE DEPARTMENT(
	ID INT PRIMARY KEY,
	DEPT CHAR(50) NOT NULL,
	EMP_ID INT references COMPANY(ID)
);
```



### CHECK 约束

CHECK 约束保证列中的所有值满足某一条件，即对输入一条记录进行检查，如果条件值为 False，则记录违反了约束，且不能输入到表。

使用方法如下：

```mysql
// 创建 COMPANY 表，并添加 5 个字段。为 SALARY 字段添加 CHECK 约束，工资不得小于0
CREATE TABLE COMPANY(
	ID INT PRIMARY KEY,
	NAME TEXT NOT NULL,
	AGE INT NOT NULL,
	ADDRESS CHAR(50),
	SALARY REAL CHECK(SALARY > 0)
);
```



### EXCLUSION 约束

使用方法如下：

```mysql
// 创建 COMPANY 表，并添加 5 个字段，并且使用 EXCLUSION 约束。
CREATE TABLE COMPANY2(
	ID INT PRIMARY KEY,
	NAME TEXT NOT NULL,
	AGE INT NOT NULL,
	ADDRESS CHAR(50),
	SALARY REAL,
	EXCLUDE USING gist
	(NAME WITH =,     --如果满足 NAME 相同， AGE 不相同则不允许插入，否则允许插入。
	AGE WITH <>)      --如果整个表达式返回 True 则不允许插入，否则允许。
);
```



### 删除约束

删除约束需要知道约束的名称。如果不知道约束的名称，则需要找到系统生成的名称，使用 **\d 表名** 可以找到这些信息。

删除约束方法如下：

```mysql
ALTER TABLE table_name DROP CONSTRAINT some_name;
```



## 连接

JOIN 子句可以把来自两个或多个表的行结合起来，基于这些表之间的共同字段。

在 PostgreSQL 中，JOIN 有五种连接类型：

CROSS JOIN ：交叉连接， INNER JOIN ： 内连接， LEFT OUTER JOIN ：左外连接， RIGHT OUTER JOIN ： 右外连接， FULL OUTER JOIN ：全外连接

### 交叉连接

交叉连接表示把第一个表的每一行与第二个表的每一行进行匹配。如果两个输入表分别有 x 行和 y 行，则结果有 x * y 行。

使用方法如下：

```mysql
SELECT ... FROM table1 CROSS JOIN table2;
```

### 内连接

内连接根据连接谓词结合两个表（table1和table2）的列值来创建一个新的结果表。查询会把 table1 中的每一行与 table2 中的每一行进行比较，找到所有满足连接谓词的行的匹配对。

**内连接是最常见的连接类型，是默认的连接类型，其中 INNER 关键字是可选的。**

使用方法如下：

```mysql
SELECT table.column1, table2.column2...
FROM table1
INNER JOIN table2
ON table1.common_field = table2.common.field;
```

### 左外连接

外部连接是内部连接的扩展。 对于外部连接，首先执行一个内部连接，然后对于表二中不满足表一中连接条件的每一行，保留表一中的全部内容，未匹配的值用 NULL 值表示。

```mysql
SELECT ... FROM table1 LEFT OUTER JOIN table2 ON conditional_expression;
```

### 右外连接

首先执行一个内部连接，对于表一中不满足表二中连接条件的每一行，保留表二中的全部内容，未匹配的值用 NULL 值表示。

```mysql
SELECT ... FROM table1 RIGHT OUTER JOIN table2 ON conditional_expression;
```

### 全外连接

一种**左外连接**和**右外连接**的结合。使用方法如下：

```mysql
SELECT ... FROM table1 FULL OUTER JOIN table2 ON conditional_expression;
```



## UNION 操作符

...



## NULL 值

NULL 值代表遗漏的未知数据。 NOT NULL 表示强制字段始终包含值，如果不向字段添加值，就无法插入新的记录或者更新记录。

**在查询数据时，NULL值可能会导致一些问题，因为一个未知的值与其他任何值比较，结果永远是未知的。**

另外，无法比较 NULL 和 0，因为他们并不等价。

可以设置查询条件为 IS NOT NULL 来查询某字段值不为空的数据。



## PostgreSQL 别名

在我们的 SQL 语句中可以重命名一张表或一个字段的名称，这个名称就是该表或该字段的别名。

创建别名是为了让表名或列名的可读性更强。

使用 AS 关键字来创建别名。

使用方法如下：

```MYSQL
// 表的别名
SELECT column1, column2...
FROM table_name AS alias_name
WHERE [ conditions ];

// 列的别名
SELECT column_name AS alias_name
FROM table_name
WHERE [ conditions ];
```



##  触发器

触发器是数据库的回调函数，它会在指定的数据库事件发生时自动执行/调用。下面是关于 PostgreSQL 触发器几个比较重要的点：

- PostgreSQL 触发器可以在下面几种情况下触发：
  - 在执行操作之前（在检查约束并尝试插入、更新或删除之前）。
  - 在执行操作之后（在检查约束并插入、更新或删除完成之后）。
  - 更新操作（在对一个视图进行插入、更新、删除时）
- 触发器的 FOR EACH ROW 属性是可选的，如果选中，当操作修改时没行调用一次；相反，选中 FOR EACH STATEMENT，不管修改多少行，每个语句标记的触发器执行一次。
- 如果存在 WHEN 子句，PostgreSQL 语句只会执行 WHEN 子句成立的那一行，如果没有 WHEN 子句，PostgreSQL 语句会在每一行执行。
- BEFORE 和 AFTER 关键字决定何时执行触发器动作，决定是在关联行的插入、修改或删除之前或者之后执行触发器动作。
- 要修改的表必须存在于同一数据库中，作为触发器被附加的表或视图，必须使用 tablename，而不是 databse.tablename
- 当创建约束触发器时会指定约束选项。这与常规触发器相同，只是可以使用这种约束来调整触发器触发的时间，当约束触发器实现的约束被违反时，会抛出异常。



使用语法：

```mysql
CREATE TRIGGER trigger_name [BEFORE|AFTER|INSTEAD OF] event_name
ON table_name
[
	--触发器逻辑...
];
```

在这里，event_name 可以是在所提到的表 table_name 上的 INSERT、DELETE 和 UPDATE 数据库操作。可以在表名后选择指定 FOR EACH ROW 。

以下是 UPDATE 操作表上的一个或多个指定列创建触发器的语法：

```mysql
CREATE TRIGGER trigger_name [BEFORE|AFTER] UPDATE OF column_name
ON table_name
[
	--触发器逻辑...
];
```

...





### PostgreSQL 索引

索引有助于加快 SELECT 查询和 WHERE 子句，但它会减慢使用 UPDATE 和 INSERT 语句时的数据输入。索引可以创建或删除，但是不会影响数据。

使用 CREATE INDEX 语句创建索引，它允许命名索引，指定表及要索引的一列或多列，并指示索引是升序排列还是降序排列。

CREATE INDEX 的语法如下：

```mysql
CREATE INDEX index_name ON table_name;
```

### 索引类型

#### 单列索引

单列索引是一个只基于表的一个列上创建索引，使用方法如下：

```mysql
CREATE INDEX index_name ON table_name(column_name);
```

#### 组合索引

组合索引是基于表的多列上创建的索引，使用方法如下：

```mysql
CREATE INDEX index_name ON table_name(column1_name, column2_name);
```

#### 唯一索引

唯一索引不允许任何重复的值插入到表中，使用方法如下：

```mysql
CREATE UNIQUE INDEX index_name ON table_name(column_name);
```

#### 局部索引

局部索引是在表的子集上构建索引，子集由一个条件表达式上定义。索引只包含满足条件的行，使用方法如下：

```mysql
CREATE INDEX index_name
ON table_name ( conditions );
```

#### 隐式索引

隐式索引是在创建对象时，由数据库服务器自动创建的索引。索引自动创建为主键约束和唯一约束。



**使用 \d table_name 命令可以查看 table_name 表下的所有索引**



### 删除索引

删除索引的使用方法为：（不知道index_name 可以使用 **\d table_name** 命令查看）

```mysql
DROP INDEX index_name;
```

### 什么情况下要避免使用索引？

索引的目的是提高读取数据的性能。但是以下几种情况应该避免使用索引：

- 索引不应该用在较小的表上。
- 索引不应该使用在有频繁的大批量的更新或插入操作的表上。
- 索引不应该使用在含有大量 NULL 值的列上。
- 索引不应该使用在频繁操作的列上。



## ALTER TABLE 命令

ALTER TABLE 命令用于添加、修改、删除一张已经存在表的列。也可以用 ALTER TABLE 命令添加和删除约束。

使用方法如下：

```mysql
// 用 ALTER TABLE 在一张已经存在的表上添加列
ALTER TABLE table_name ADD column_name dataType;

// 用 ALTER TABLE 在一张已经存在的表上删除列
ALTER TABLE table_name DROP COLUMN column_name;

// 用 ALTER TABLE 修改表中某列的 数据类型 
ALTER TABLE table_name ALTER COLUMN column_name TYPE dataType;

// 用 ALTER TABLE 给表中某列添加 NOT NULL 约束
ALTER TABLE table_name MODIFY column_name dataType NOT NULL;

// 用 ALTER TABLE 给表中某列添加 UNIQUE 约束
ALTER TABLE table_name ADD CONSTRAINT MyUniqueConstraint UNIQUE(column1, column2...);

// 用 ALTER TABLE 给表中某列添加 CHECK 约束
ALTER TABLE table_name ADD CONSTRAINT MyCheckConstraint CHECK(condition);

// 用 ALTER TABLE 给表中某列添加 主键 约束
ALTER TABLE table_name ADD CONSTRAINT MyPrimaryKey PRIMARY KEY (column1, column2...);

// 用 ALTER TABLE 删除约束
ALTER TABLE table_name DROP CONSTRAINT constraint_name;
```



## TRUNCATE TABLE

TRUNCATE TABLE 用于删除表的数据，但是不删除表的结构。使用 DROP TABLE 也能够删除表， 但是会连同表的结构一起删除，如果想插入数据需要重新建立这张表。

TRUNCATE TABLE 与 DELETE TABLE 具有相同的效果，但是 TRUNCATE 实际上并不扫描表，所以速度更快。 TRUNCATE 能够立即释放空间，不需要后续的 VACUUM 操作（1. 被删除或者被更新废弃的元组并没有在物理上从它们的表中移除，它们将一直存在直到一次 VACUUM 操作。 2. 这在大型表中非常有用）

使用方法：

```mysql
TRUNCATE TABLE table_name;
```



## 视图





## 事务 TRANSACTION （如何实现的）

TRANSACTION （事务）是数据库管理系统执行过程中的一个逻辑单位，由一个有限的数据库操作序列构成。

数据库事务通常包含了对数据对读/写操作的一个序列，包含以下两个目的：

- 为数据库操作序列提供了一个从失败中恢复到正常状态的方法，同时提供了数据库即使在异常状态下仍能保持一致性的方法。
- 当多个应用程序在并发访问数据库时，可以在这些应用程序之间提供一种隔离方法，以防止彼此的操作互相干扰。



事务的属性：ACID

**事务的控制：使用下面的命令来控制事务：**

- BEGIN TRANSACTION 开始一个事务
- COMMIT ：事务确认，或者可以使用 END TRANSACTION 命令
- ROLLBACK ： 事务回滚

事务控制命令只与 INSERT、 UPDATE、 DELETE 一起使用。不能在创建表和删除表时使用，因为这些操作在数据库中是自动提交的。



## PostgreSQL 锁 （LOCK）

锁主要是为了保护数据库的一致性，可以阻止用户修改一行或者整个表，一般用在并发较高的数据库中。在多个用户访问数据库的时候若对并发操作不加控制就可能会读取和存储不正确的数据，破坏数据库的一致性。数据库中有两种基本的锁：排他锁（Exclusive Locks）和共享锁（Share Locks）。如果数据对象加上排他锁。则其他的事务不能对它读取和修改。如果加上共享锁，则数据库对象可以被其他事务读取，但不能修改。

LOCK 命令使用方法：

```mysql
LOCK TABLE table_name IN lock_mode;
```

table_name : 要锁定的现有表的名称。
lock_mode : 锁定模式，指定该锁与哪个锁冲突。如果没有指定锁模式，则**使用限制最大的访问独占模式**，可能的值有：ACESS SHARE, ROW SHARE, ROW EXCLUSIVE, SHARE UPDATE EXCLUSIVE, SHARE, SHARE ROW EXCLUSIVE, EXCLUSIVE, ACCESS EXCLUSIVE.

一旦获得了锁，锁将在当前事务的其余时间保持。没有解锁的命令。锁总是在事务结束时释放。



## PostgreSQL 子查询

子查询也称为内部查询、嵌套查询，指的是在 PostgreSQL 查询中的 WHERE 子句中嵌入查询语句。一个 SELECT 语句的查询结果能够作为另一个语句的输入值。子查询可以与 SELECT、INSERT、UPDATE 和 DELETE 语句一起使用，并可以搭配运算符一起使用。

子查询必须遵循的几个规则：

- 子查询必须用括号括起来。
- 子查询在 SELECT 子句中只能有一个列，除非主查询中有多列，与子查询所选列进行比较。
- ORDER BY 不能用在子查询中。GROUP BY 可以在子查询中使用。
- 子查询返回多于一行，只能与多值运算符一起使用，如 IN 运算符。
- BETWEEN 运算符不能与子查询一起使用，但是， BETWEEN 可在子查询内使用。