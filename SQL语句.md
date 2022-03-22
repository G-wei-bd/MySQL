# SQL语句

## SQL基本命令
### cmd命令
```cmd
# 运行mysql
net start mysql80

# 停止mysql
net stop mysql80

# cmd 连接mysql
mysql [-h 127.0.0.1] [-P 3306] -u root -p
```
### `DDL` 数据定义语言
用来定义数据库对象（数据库，表，字段）
```SQL
1.创建数据库
    CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
2.使用数据库
    USE DATABASE 数据库名;
3.查询所有数据库
    SHOW DATABASES;
4.查询当前数据库
    SELECT DATABASE();
5.删除数据库
    DROP DATABASE [IF EXISTS] 数据库名;
6.查询当前数据库所有表
    SHOW TABLES;
```
```sql 
1.创建表
    CREATE TABLE 表名(
        字段1 字段1类型[COMMENT 字段1注释],
        字段2 字段2类型[COMMENT 字段2注释],
        字段3 字段3类型[COMMENT 字段3注释],
        ······
        字段n 字段n类型[COMMENT 字段n注释]
    )[COMMENT 表注释];
2.添加字段
    ALTER TABLE 表名 ADD 字段名 类型(长度) [COMMENT 注释] [约束];
3.查询表结构
    DESC 表名;
4.查询指定表的建表语句
    SHOW CREATE TABLE 表名;
5.修改数据类型
    ALTER TABLE 表名 MODIFY 字段名 新数据类型(长度);
6.修改字段名和字段类型
    ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型(长度) [COMMENT 注释] [约束];
7.删除字段
    ALTER TABLE 表名 DROP 字段名;
8.修改表名
    ALTER TABLE 表名 RENAME TO 新表名;
9.删除表
    DROP TABLE [IF EXISTS] 表名;
10.删除指定表，并重新创建该表
    TRUNCATE TABLE 表名;
```
### `DML` 数据操作语言
用来对数据库表中的数据进行增删改
```sql
1.给指定字段添加数据
    INSERT INTO 表名(字段名1,字段名2,...) VALUES (值1,值2,...);
2.给全部字段添加数据
    INSERT INTO 表名 VALUES (值1,值2,...);
3.批量添加数据
    INSERT INTO 表名(字段名1,字段名2,...) VALUES (值1,值2,...),(值1,值2,...),(值1,值2,...);
    INSERT INTO 表名 VALUES (值1,值2,...) (值1,值2,...) (值1,值2,...);
    // 注意; 1.插入数据时，指定的字段顺序需要与值的顺序是一一对应的；
            2.字符串和日期型数据应该包含在引号中；
            3.插入的数据大小，应该在字段的规定范围内 
4.修改数据
    UPDATE 表名 SET 字段名1=值1,字段名2=值2,...[WHERE 条件];
    // 注意：如果不带 where 条件则更新所有表中符合字段名的数据
5.删除数据
    DELETE FROM 表名 [WHERE 条件];
    // DELETE 语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据
    // DELETE 语句不能删除某一个字段的值(可以使用UPDATE)
```
### `DQL` 数据查询语言
用来查询数据库中表的记录
```sql
# SQL 语句编写顺序                  执行顺序
SELECT          字段列表               4 
FROM            表名列表               1
WHERE           条件列表               2
GROUP BY        分组字段列表           3
HAVING          分组后条件列表      
ORDER BY        排序字段列表           5
LIMIT           分页参数               6
```

```sql
# 基本查询
1.查询多个字段
    SELECT 字段1,字段2,字段3 ... FROM 表名;
    SELECT * FROM 表名;
2.设置查询字段别名
    SELECT 字段1 [AS 别名1],字段2 [AS 别名2] ... FROM 表名;
3.去除重复记录
    SELECT DISTINCT 字段列表 FROM 表名;

# 条件查询
    SELECT 字段列表 FROM 表名 WHERE 条件列表;
    // 可用条件有 >, >=, <, <=, =, <>或 !=, BETWEEN TO(某个范围之内(包含最小，最大值)), IN(...)(在in之后的列表中的值，符合任意一个都查询), LIKE 占位符(模糊匹配(_匹配单个字符，%匹配任意个字符)), IS NULL (是null), AND 或 &&, OR 或 ||, NOT 或 !
# 排序查询
    SELECT 字段列表 FROM 表名 ORDER BY 字段1 排序方式1,字段2,排序方式2;
    // 排序方式：ASC 升序，DESC 降序。如果是多字段排序，当第一个字段值相同时才会根据第二个字段进行排序。
# 分页查询
    SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数;
    // 起始索引从 0 开始，起始索引 = (查询页码 - 1) * 每页显示记录数。
    // 分页查询是数据库的方言，不同的数据库有不同的实现，MySQL 中是 LIMIT。
    // 如果查询的是第一页数据，起始索引可以省略，直接简写为 LIMIT 10。
```
```sql
聚合函数
// 将一列数据作为一个整体进行纵向计算
常见聚合函数
    COUNT       统计数量
    MAX         最大值
    MIN         最小值
    AVG         平均值
    SUM         求和
语法
    SELECT 聚合函数(字段列表) FROM 表名;
```
### `DCL` 数据控制语言
用来创建数据库用户、控制数据库的访问权限
```SQL
# 查询用户
    USE mysql;
    SELECT * FROM user;
# 创建用户
    CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
# 修改用户密码
    ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
# 删除用户
    DROP USER '用户名'@'主机名';
```
```sql
# 权限控制
    ALL, ALL PRIVILEGES     所有权限
    SELECT                  查询数据
    INSERT                  插入数据
    UPDATE                  修改数据
    DELETE                  删除数据
    ALTER                   修改表
    DROP                    删除数据库/表/视图
    CREATE                  创建数据库/表

# 查询权限
    SHOW GRANTS FOR '用户名'@'主机名';
# 授予权限
    GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
# 撤销权限
    REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

## 函数
### 用法 : `SELECT 函数(参数);`
### 字符串函数
```SQL
    CONCAT(S1, S2, ..., Sn)         字符串拼接，将 S1, S2, ..., Sn 拼接成一个字符串
    LOWER(str)                      将字符串 str 全部转为小写
    UPPER(str)                      将字符串 str 全部转为大写
    LPAD(str, n, pad)               左填充，用字符串 pad 对 str 的左边进行填充，达到 n 个字符长度
    RPAD(str, n, pad)               右填充，用字符串 pad 对 str 的右边进行填充，达到 n 个字符长度
    TRIM(str)                       去掉字符串头部和尾部的空格
    SUBSTRING(str, start, len)      返回从字符串 str 从 start 位置起的 len 个长度的字符串
```
### 数值函数
```sql
    CEIL(X)         向上取整
    FLOOR(X)        向下取整
    MOD(X, Y)       返回 X/Y 的模
    RAND()          返回 0-1 内的随机数
    ROUND(X, Y)     求参数 X 的四舍五入的值，保留 Y 位小数
```

### 日期函数
```SQL
    CURDATE()                               返回当前日期
    CURTIME()                               返回当前时间
    NOW()                                   返回当前日期和时间
    YEAR(date)                              获取指定 date 的年份
    MONTH(date)                             获取指定 date 的月份
    DAY(date)                               获取指定 date 的日期
    DATE_ADD(date, INTERVAL expr type)      返回一个日期/时间值加上一个事件间隔 expr 后的时间值
    DATEDIFF(date1, date2)                  返回起始时间 date1 和结束时间 date2 之间的天数
```

### 流程函数 
```SQL
IF(value, t, f)                                             如果 value 为 true，则返回 t，否则返回 f
IFNULL(value1, value2)                                      如果 value1 不为空，返回 value1，否则返回 value2
CASE WHEN [val1] THEN [res1] ... ELSE [default] END         如果 val1 为 true，则返回 res1，... 否则返回 default 默认值
CASE [expr] WHEN [val1] THEN [res1] ... ELSE [default] END  如果 expr 的值等于 val1，返回 res1，... 否则返回 default 默认值
```

## 约束
### 使用的约束
```sql
NOT NULL        非空约束
UNIQUE          唯一约束
PRIMARY KEY     主键约束
DEFAULT         默认约束
CHECK           检查约束
FOREIGN KEY     外键约束
```
### 外键约束
```SQL
// 外键用来让两张表的数据之间建立连接，从而保证数据的一致性和完整性
# 添加外键约束
    ## 创建表时添加
    CREATE TABLE 表名(
        字段名 数据类型
        ...
        [CONSTRAINT] [外键名称] FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);
    );

    ## 创建表后添加
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名);

# 删除外键
    ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;

# 外键删除/更新行为
    NO ACTION           当在父表删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新(默认行为)   
    RESTRICT            当在父表删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新(默认行为)
    CASCADE             当在父表删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则也删除/更新外键在子表中的记录
    SET NULL            当在父表删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为 null (要求该外键允许取 null 值)
    SET DEFAULT         父表有变更时，子表将外键列设置成一个默认的值(Innodb 不支持)
## 语法
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名) REFERENCES 主表(主表列名) ON UPDATE 行为 ON DELETE 行为;
```

## 多表查询
### 1.多表关系
```SQL
    一对多(多对一)           在多的一方建立外键，指向一的一方的主键
    多对多                  建立第三张中间表，中间表至少包含两个外键，分别关联两方主键
    一对一                  在任意一方加入外键，关联另外一方的主键，并且设置外键为唯一的(UNIQUE)
```
### 2.多表查询
    在利用 SELECT 查询多张表时，会出现许多无用的数据集合，需要消除无效的笛卡尔积，在查询的时候利用条件进行筛选即可消除笛卡尔积。
    # 多表查询的分类
        1.连接查询
            内连接：相当于查询A, B 交集部分的数据
            外连接：
                左外连接：查询左表所有数据，以及两张表交集部分数据
                右外连接：查询右表所有数据，以及两张表交集部分数据
            自连接：当前表与自身的连接查询，自连接必须使用表别名
        2.子查询
            标量子查询，行子查询，列子查询，表子查询

#### 连接查询
```SQL
    1.内连接
        # 隐式内连接
        SELECT 字段列表 FROM 表1, 表2 WHERE 条件...;
        # 显式内连接
        SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件...;
    2.外连接
        # 左外连接
        SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件...;
        # 右外连接
        SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件...;
    3.自连接(可以是外连接，也可以是内连接)
        SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件...;
```

#### 联合查询
        // 把多次查询的结果合并起来，形成一个新的查询结果集
            SELECT 字段列表 FROM 表A ...
            UNION [ALL]
            SELECT 字段列表 FROM 表B ...;
        # 注意：
        1. 对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致
        2. UNION ALL 是将多个表查询的结果进行合并，不会去重，而 UNION 是将查询出来的结果进行去重再合并

#### 子查询
```SQL
    // SQL 语句中嵌套 SELECT 语句，称为嵌套查询，又称子查询。子查询外部的语句可以是 INSERT / UPDATE / DELETE / INSERT 的任何一个
    // 根据子查询位置，分为：WHERE 之后，FROM 之后，SELECT 之后
    1.标量子查询(子查询结果为单个值)
        返回的结果是单个值(数字，字符串，日期等)，
        常用的操作符有 = , <>, > , >= , < , <=

    2.列子查询(子查询结果为一列)
        返回的结果是一列(可以是多行)，
        常用的操作符有 IN(在指定的集合范围内多选一), NOT IN(不在指定的集合范围之内), ANY(在返回列表中有任意一个满足即可), SOME(与 ANY 等同), ALL(返回列表所有的值都必须满足)

    3.行子查询(子查询结果为一行)
        返回的结果是一行(可以是多列)，常用的操作符有 =, <>, IN, NOT IN

    4.表子查询(子查询结果为多行多列)
        返回的结果是多行多列，常用的操作符有 IN
```

## 事务
### 事务简介
    事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，默认 MYSQL 的事务是自动提交的
### 事务四大特性
    原子性，一致性，隔离性，持久性
### 事务语法
```sql
    # 查看/设置事务提交方式(0 表示手动提交，1 表示自动提交)
        SELECT @@autocommit;
        SET @@autocommit = 0;
    # 提交事务
        COMMIT;
    # 回滚事务
        ROLLBACK;
    # 开启事务
        START TRANSACTION 或 BEGIN;
```
### 事务隔离
|  问题  |  描述  | 
|   :----:  | :----: |
|  赃读 | 一个事务读到另一个事务还没有提交的数据 |
|  不可重复读 | 一个事务向后读取同一条记录，但两次读取的数据不同，称之为不可重复读 |
|   幻读 | 一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了“幻影” |

### 事务隔离级别
|  隔离级别  |  赃读  |  不可重复读 |   幻读   |
|   :----:  | :----: | :---------: | :-----: |
|  Read uncommitted | √ | √ | √ |
|  Read committed | × | √ | √ |
|   Repeatable Read(默认) | × | × | √ |
|  Serializable | × | × | × |

### 查看/设置事务隔离级别
```sql
    # 查看事务隔离级别
        SELECT @@TRANSACTION_ISOLATION;
    # 设置事务隔离级别
        SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL {READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
```
 注意：事务隔离级别越高，数据越安全，但是性能越低 
