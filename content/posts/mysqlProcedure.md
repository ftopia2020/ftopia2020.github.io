---
title: MySQL 存储过程的简单使用
date: 2019-10-12
author: "me"
tags: ["MySQL", "原创"]
categories: ["后端"]
---


## Why

如果你是数据库老手，对存储过程应该不陌生，在商业数据库应用中，例如金融、企业、政府等等，存储过程的使用非常广泛。然而，MySQL 存储过程的功能相较 Oracle等商用数据库来说很弱，且性能也差。

那用 MySQL 存储过程干嘛呢？在个人工作中，难免需要拉点数据、批量创建或更改些数据（通常是本地测试）。当然，代码直写也可，像是 Python 或是新宠 Go 都还算方便，但是如果只是 SQL语句+变量或逻辑控制，直写 SQL 语句因其不需要语言环境就方便得多，且团队中复用性强（不会写存储过程的产品经理不是好的 DBA，啊哈哈）。

然而，个人不赞成将 MySQL存储过程用于项目中，尤其是迭代快的互联网应用，参考如下：

[存储过程在实际项目中用的多吗？- From 知乎](https://www.zhihu.com/question/54408187/answer/139275947)

[为什么阿里巴巴Java开发手册里要求禁止使用存储过程？](https://www.zhihu.com/question/57545650/answer/325422160)

所以，本文只是介绍 「如何简单使用 MySQL 存储过程」而已。

> 对于批量创建/更改测试数据，存储过程真的很方便啊 ~
>
> 我五年前做系统测试，还能看看写写 SQL Server 的冗长存储过程和视图（原东家大型.net电商网站，10+表JOIN 那酸爽你懂的），虽然现在该技能已被磨成 0

## What

MySQL 5.0 版本开始支持存储过程。

存储过程（Procedure）完成了特定功能的SQL语句集，经编译创建并保存在数据库中，用户可通过指定存储过程的名字并给定参数(需要时)来调用执行。

存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装与重用。

## How

下面将具体介绍 **创建存储过程、调用存储过程**，存储过程语句中用到的**变量、条件及循环语句**也会简单提及。

### 创建存储过程

官方文档：[CREATE PROCEDURE and CREATE FUNCTION Syntax](https://dev.mysql.com/doc/refman/8.0/en/create-procedure.html)

以下为官方示例，(运行前提需要存在 t 表)

```shell
mysql> delimiter //

mysql> CREATE PROCEDURE simpleproc (OUT param1 INT)
    -> BEGIN
    ->   SELECT COUNT(*) INTO param1 FROM t;
    -> END//
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter ;
```

基础语法剖析如下（官方语法简化）：

```mysql
DELIMITER //
 CREATE PROCEDURE sp_name ([proc_parameter[,...]])
   BEGIN
　　[statement_list]
　　　　……
     END //
DELIMITER ;
```

1. 外层 `DELIMITER //` 更改分隔符为 // `END //`表示结束 `DELIMITER ;` 将将分隔符更改回分号

2. `CREATE PROCEDURE` 语句创建新的存储过程，语句之后指定存储过程名称，括号内可指定输入、输出参数，参数可定义如下：官方示例指定输出为 INT 类型的 param1 变量

   ```
   proc_parameter:
       [ IN | OUT | INOUT ] param_name type
   ```

3. `BEGIN` 和 `END` 之间为存储过程主体，将声明性 SQL 语句放在主体中处理业务逻辑。示例中将 t 表数量返回。

在 MySQL客户端工具中编写存储过程非常繁琐，特别是当存储过程复杂时。 大多数用于MySQL的GUI工具允许您通过直观的界面创建新的存储过程。以 Navicat 为例，functions 右键可直接快捷创建，生成简单示例如下：

![mysql_navicat_proc_sample](http://image.ftopia.cn/blog/mysql_navicat_proc_sample.png)

![mysql_navicat_proc_sample2](http://image.ftopia.cn/blog/mysql_navicat_proc_sample2.png)

工具使用上应该不难理解，窗体按钮可操作保存、执行等，下方可定义参数及返回类型。

### 调用存储过程

调用存储过程，可使用以下 SQL命令：

```
CALL sp_name ([proc_parameter[IN]])
mysql> CALL simpleproc(@a);
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @a;
+------+
| @a   |
+------+
| 3    |
+------+
1 row in set (0.00 sec)
```

示例执行如上，客户端的执行通常只要点击按钮即可。

客户端创建、查看及修改存储过程比较方便，不是很建议命令行操作，实际上即使项目中应用存储过程，研发中也是先使用工具在测试环境编写及测试，然后将测试过的存储过程进行发布的。

### 变量

变量是一个命名数据对象，变量的值可以在存储过程执行期间更改。

> 注意：变量必须先声明后，才能使用它。

要在存储过程中声明一个变量，可以使用 `DECLARE`语句，使用 `SET` 进行变量赋值，如下所示：

```mysql
DECLARE variable_name datatype(size) DEFAULT default_value;
SET variable_name = set_value;
BEGIN
    DECLARE total_count INT DEFAULT 0;
    SET total_count=10;
    SELECT total_count;
END
```

尝试以上示例并保存，调用后如下：

```
mysql> call simpleproc();
+-------------+
| total_count |
+-------------+
|          10 |
+-------------+
1 row in set (0.01 sec)
```

一个变量有自己的范围(作用域)，它用来定义它的生命周期。 如果在存储过程中声明一个变量，那么当达到存储过程的 `END` 语句时，它将超出范围，因此在其它代码块中无法访问。

以 `@` 符号开头的变量是会话变量。直到会话结束前它可用和可访问，如最初的示例中 `@a`。

存储过程中可加入使用 SQL 语法的流程控制，像是条件判断 IF / CASE、WHILE 循环等，下面将对此进行一些简单的介绍。

### 条件判断

假设 t 表中数据如下：

```shell
mysql> select * from t;
+----+-------+-------+
| Id | Name  | Score |
+----+-------+-------+
|  1 | John  |    78 |
|  2 | Macy  |    90 |
|  3 | Mary  |    85 |
|  4 | Ted   |    97 |
|  5 | Ada   |    65 |
|  7 | Peter |    92 |
+----+-------+-------+
6 rows in set (0.00 sec)
```

如果我们需要统计>=85分的人数，如果人数多于总数的一半，则评价为 A，否则为 B，这个需求用代码完成不算困难，用 MySQL 存储过程也是如此，参考如下：

```mysql
BEGIN
    DECLARE result CHAR(1);
    DECLARE highCount INT DEFAULT 0;
    DECLARE totalCount INT DEFAULT 0;
    SELECT count(*) INTO totalCount FROM t;
    SELECT count(*) INTO highCount FROM t WHERE Score>=85;
    SET result = 'B';
    IF highCount>(totalCount/2) THEN
        SET result = 'A';
    END IF;
    SELECT totalCount/2 AS 'halfCount', highCount, result;
END
```

你可以先不看下文，猜测下调用结果会是如何？

```
mysql> call simpleproc();
+-----------+-----------+--------+
| halfCount | highCount | result |
+-----------+-----------+--------+
|    3.0000 |         4 | A      |
+-----------+-----------+--------+
1 row in set (0.01 sec)
```

如果你跟着示例进行测试，可以增改数据再尝试下

CASE 语句这边就不提了，感觉就是 IF ELSE 的简单写法，最下方参考教程里面有。

### 循环语句

循环可是创建测试数据的利器，不可不会系列，接上例，如果我们需要批量创建如 100 条名称及成绩数据，成绩 < =100，你会如何实现？使用 MySQL 存储过程试下吧，参考如下：

```mysql
BEGIN
    DECLARE i INT DEFAULT 0;
    WHILE i<100 DO  
        INSERT INTO sample.t (`NAME`, `Score`)VALUES(CONCAT('SNAME_', CAST(i AS CHAR)), RAND()*100);
        SET i=i+1;
    END WHILE;
END
```

新建存储过程，然后调用看下吧~ 之后查询 t 表结果如下（过长省略）：

```
mysql> select * from t limit 100;
+-----+----------+-------+
| Id  | Name     | Score |
+-----+----------+-------+
|   1 | John     |    78 |
|   2 | Macy     |    90 |
|   3 | Mary     |    85 |
|   4 | Ted      |    97 |
|   5 | Ada      |    65 |
|   7 | Peter    |    92 |
|   8 | SNAME_0  |    51 |
|   9 | SNAME_1  |    75 |
|  10 | SNAME_2  |    23 |
|  11 | SNAME_3  |    90 |
|  12 | SNAME_4  |    82 |
|  13 | SNAME_5  |    38 |
|  14 | SNAME_6  |    44 |
|  15 | SNAME_7  |     7 |
|  16 | SNAME_8  |     3 |
|  17 | SNAME_9  |    94 |
……
```

示例相对简单（初学者需要适当 Google 一下里面的函数使用），实际使用上语句会更复杂，且会加入更多函数或计算等，本文就不深入研究了，自行探索吧~

### 参考

本文只是简单介绍 MySQL 存储过程的使用，如果你想了解更多，建议参考：

- [MySQL教程——第三章 存储过程](https://necan.gitbooks.io/mysql-tutorial/content/存储过程/简介.html)
- [runoob.com MySQL 存储过程](https://www.runoob.com/w3cnote/mysql-stored-procedure.html)