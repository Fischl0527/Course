# 说明
`[……]`：可选项

#  基础
### SQL 启动
`mysql -u root（用户名） -h 127.0.0.1 （mysql 服务器所在地址）-p`

```sql
PS C:\Users\gaohang> mysql -u root -h 127.0.0.1 -p
Enter password: ******
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 13
Server version: 5.7.35-log MySQL Community Server (GPL)

Copyright (c) 2000, 2024, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

### SQL 退出
`quit;`

```sql
mysql> quit;
Bye
```

### 查看数据库
`show databases/schemas [like '模式' where 条件 ];`

`%`：匹配任意数量的字符。	`_`：匹配单一字符

`where` 用于 `information_schema.schemata` 表中的列

```sql
mysql> show schemas;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| challenges         |
| fischl             |
| mysql              |
| performance_schema |
| security           |
| sys                |
| text               |
+--------------------+
8 rows in set (0.00 sec)

mysql> show databases like 'f____l';
+-------------------+
| Database (f____l) |
+-------------------+
| fischl            |
+-------------------+
1 row in set (0.00 sec)

mysql> show databases like 'f%l';
+----------------+
| Database (f%l) |
+----------------+
| fischl         |
+----------------+
1 row in set (0.00 sec)

mysql> show databases like '%is%';
+-----------------+
| Database (%is%) |
+-----------------+
| fischl          |
+-----------------+
1 row in set (0.00 sec)
```

### 创建数据库
`create datacases/schema [if not exists] 数据库名 [ [default]character set = 字符集]`

```sql
mysql> create database if not exists fischl
    -> character set = GBK;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| challenges         |
| fischl             |
| mysql              |
| performance_schema |
| security           |
| sys                |
| text               |
+--------------------+
```

### 选择数据库
`use 数据库名;`

```sql
mysql> use fischl;
Database changed	/*表示选中该数据库*/
```

### 修改数据库
`alter database/schema [数据库名] [default] character set = 字符集`

```sql
mysql> alter database fischl
    -> character set = gbk;
Query OK, 1 row affected (0.01 sec)
```

### 删除数据库
`drop database/schema if exists 数据库名` 

```sql
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| challenges         |
| fischl             |
| klee               |
| mysql              |
| performance_schema |
| security           |
| sys                |
| text               |
+--------------------+
9 rows in set (0.00 sec)

mysql> drop database if exists klee;
Query OK, 0 rows affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| challenges         |
| fischl             |
| mysql              |
| performance_schema |
| security           |
| sys                |
| text               |
+--------------------+
8 rows in set (0.00 sec)
```

### 查看引擎
`show ebgines;`

```sql
mysql> show engines;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```

<details class="lake-collapse"><summary id="u6d08a697"><span class="ne-text">表头说明</span></summary><p id="ubd5711d3" class="ne-p"><span class="ne-text">Engine：存储引擎的名称。</span></p><p id="u66e535a5" class="ne-p"><span class="ne-text">Support：服务器对该引擎的支持情况。DEFAULT 表示默认引擎，YES 表示已启用，NO 表示未启用或未编译，DISABLED 表示已禁用。</span></p><p id="u77657064" class="ne-p"><span class="ne-text">Comment：引擎的简短描述。</span></p><p id="u06a97280" class="ne-p"><span class="ne-text">Transactions：是否支持事务（YES/NO）。</span></p><p id="ub6774223" class="ne-p"><span class="ne-text">XA：是否支持分布式事务（XA 事务）。</span></p><p id="u61bab456" class="ne-p"><span class="ne-text">Savepoints：是否支持保存点（事务内的回滚点）。</span></p></details>
<details class="lake-collapse"><summary id="uae88b63c"><span class="ne-text">列说明</span></summary><p id="u971dfefb" class="ne-p"><span class="ne-text">InnoDB：默认存储引擎，支持事务、行级锁和外键，适合高并发和数据一致性要求高的应用。</span></p><p id="uf9ca0e86" class="ne-p"><span class="ne-text">MRG_MYISAM（MERGE）：将多个结构相同的 MyISAM 表合并为一个逻辑表，便于分区管理。</span></p><p id="u301b2929" class="ne-p"><span class="ne-text">MEMORY：数据存储在内存中，读写极快但重启丢失，适合临时表或缓存。</span></p><p id="ubf1d9001" class="ne-p"><span class="ne-text">BLACKHOLE：写入的数据被丢弃，读取返回空，常用于复制中的中转或日志记录。</span></p><p id="udb526589" class="ne-p"><span class="ne-text">MyISAM：不支持事务和外键，使用表级锁，适合只读或低写入场景，但崩溃恢复慢。</span></p><p id="u474f6911" class="ne-p"><span class="ne-text">CSV：以CSV文件存储数据，便于与其他程序交换，不支持索引。</span></p><p id="u06e736ba" class="ne-p"><span class="ne-text">ARCHIVE：专为归档设计，压缩存储节省空间，只支持插入和查询，适合历史数据。</span></p><p id="u1a79fabe" class="ne-p"><span class="ne-text">PERFORMANCE_SCHEMA：收集服务器性能数据（如锁、语句统计），用于监控和调优。</span></p><p id="ufa492d79" class="ne-p"><span class="ne-text">FEDERATED：允许访问远程MySQL表（需启用），类似链接表。</span></p></details>
# 操作数据表
