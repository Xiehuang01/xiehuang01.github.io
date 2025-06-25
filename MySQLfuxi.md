# MySQL数据库

# 1、数据库操作

### 1.创建数据库

~~~sql
CREATE DATABASE 数据库名称;
~~~



### 2.查看数据库

~~~sql
show databases;
~~~



### 3.使用数据库

~~~sql
USE 数据库名称;
~~~



### 4.删除数据库

~~~sql
DROP DATABASE 数据库名称;
~~~



# 2、对表的操作

**前提：先使用对应的数据库**

### 1.创建表

~~~sql
CREATE table 表名称(
	字段名(表头名) 数据类型 [NOT NULL] [PRIMARY KEY],
	字段名(表头名) 数据类型,
    ......
    字段名(表头名) 数据类型
);
~~~

NOT NULL：不允许为空	PRIMARY KEY：作为唯一的值（主键）



### 2.查看表结构

~~~sql
DESC 表名称;
~~~



### 3.查看所有的表

~~~sql
SHOW TABLES;
~~~



### 4.删除表

~~~sql
DROP TABLE 表名称;
~~~



# 3.对表字段(列)结构的增删改

### 1.增加字段add

~~~sql
ALTER TABLE 表名称
	  ADD 字段名(表头名) 数据类型;
~~~



### 2.删除字段drop

~~~sql
ALTER TABLE 表名称
	 DROP 字段名(表头名);
~~~



### 3.修改字段类型 modify

~~~sql
ALTER TABLE 表名称
	  MODIFY 字段名(表头名) 新的数据类型;
~~~



### 4.重命名字段名 change

~~~sql
ALTER TABLE 表名称
	  CHANGE 旧的字段名(表头名) 新的字段名(表头名);
~~~



# 4-1、对表数据的操作（增、删、改）

### 1-1.插入 一行 的表记录

~~~sql
insert into 表名称
	values (记录1,记录2,...,记录n);
~~~



### 1-2.插入 指定 的表记录

~~~sql
insert into 表名称(表头1,表头x)
	values (记录1,记录x);
~~~



### 2-1.删除指定行的表记录

~~~sql
delete from 表名称 where 条件（哪一行）;
~~~



### 2-2.删除所有行的表记录

~~~sql
truncate table 表名称; 或 delete from 表名称;
~~~



### 3.修改表记录的

~~~sql
update 表名称
	set 表头x=表记录x,表头y=记录y,...,表头z=记录z
	where 条件（谁要变）;
~~~



# 4-2、对表数据的操作（查）

### 1.普通查询

~~~sql
select * from 表名称;
select 字段1, 字段2, ··· from 表名称;
~~~



### 2.去掉重复行 distinct

~~~sql
select distinct */字段1, 字段2, ··· from 表名称;
~~~



### 3.取别名AS/空格

~~~sql
select 字段x AS 字段x的别名 from 表名称;
~~~



### 4-1.where条件查询

~~~sql
select 字段1, 字段2, ··· from 表名称
	where 条件;
~~~

比较运算符： <、<=、>、>=、!=或<>、=



### 4-2.查询语句的通配符 和 逻辑运算符

~~~sql
NOT AND OR (优先级: 高->低)
~~~

**！！**注意：当判断某一个值是否为NULL的时候要用 IS [NOT] NULL 而不是 = 、!= **！！**

**AND的特殊用法：**

~~~sql
判断条件A between 判断值1 and 判断值2 等于 ( 判断值1 <= A <= 判断值2)
~~~

**IN的特殊用法**

~~~sql
判断条件 in (判断值1, 判断值2) 等于 ( A=判断值1 or A=判断值2)
~~~



### 4-3.字符匹配符

- **“%” 用于表示0个或任意多个字符**
- **“_” 表示任意1个字符**

~~~sql
where A like ‘张%’;
where A like ‘张_’;
~~~



### 5.排序

**ASC：升序**

**DESC：降序**

默认为ASC

~~~sql
order by desc / asc;
~~~

**！！**注意：当SELECT语句中同时包含多个子句，如WHERE、GROUP BY、HAVING、ORDER BY ORDER BY 子句必须放至最后**！！**



### 6.聚集函数

~~~sql
count(字段)
sum(字段)
avg(字段)
max(字段)
min(字段)
~~~



### 7.分组

~~~sql
group by 字段;
~~~

**分组条件：**

~~~sql
group by 字段
	having 条件;
~~~



### 8.多表连接

#### 1.“万能”写法

~~~sql
select ... from t1, t2
	where t1.A = t2.A;
~~~

#### 2.内连接

~~~sql
select ...from t1 inner join t2
	on t1.A = t2.A;
~~~

#### 3.自连接：

~~~sql
select ...from t1 as a, t1 as b
	on a.A = b.A;
~~~

#### 4.左外连接：

~~~sql
select ...from t1 left outer join t2
	on t1.A = t2.A;
~~~

用来从两个表中提取相关数据，同时保留左表中的所有记录，即使右表中没有匹配的记录

**假设有两个表：**

`students`

| id   | name  |
| ---- | ----- |
| 1    | Tom   |
| 2    | Alice |
| 3    | Jack  |

`scores`

| student_id | score |
| ---------- | ----- |
| 1          | 90    |
| 2          | 85    |

SQL 查询：

```sql
SELECT students.id, students.name, scores.score FROM students LEFT JOIN scores
	ON students.id = scores.student_id;
```

查询结果：

| id   | name  | score |
| ---- | ----- | ----- |
| 1    | Tom   | 90    |
| 2    | Alice | 85    |
| 3    | Jack  | NULL  |

#### 5.右外连接：

~~~sql
select ...from t1 right outer join t2
	on t1.A = t2.A;
~~~

用来从两个表中提取相关数据，同时保留右表中的所有记录，即使左表中没有匹配的记录

#### 6.全外连接：

~~~sql
select ... from t1 full outer join t2
	on t1.A = t2.A;
~~~

用来从两个表中提取相关数据，显示两个表中所有记录，没有相同的记录，相应列的值为NULL



### 9.子查询(嵌套查询)

~~~sql
select ... from ...
	where A in (select ... from ...)
~~~

in 相当于 = 除了in还可以用>、< 、ANY、ALL、exists

- ANY：任意值
- ALL：全部值
- exits：该值是否存在



### 10.索引

**创建索引**

~~~sql
create [unique] index 索引名 on 表名称(字段名 [DESC/ASC]);
~~~

UNIQUE：唯一性索引

**删除索引**

~~~sql
drop index 索引名 on 表名称;
~~~

**查看索引**

~~~sql
show index from 表名称;
~~~



### 11.视图

**创建视图**

~~~sql
create  [or replace] view 视图名 as
	select ... 
~~~

使用or replace来修改视图

**使用视图**

~~~sql
select ... from 视图名;
~~~

**删除视图**

~~~sql
drop view 视图名;
~~~



# 5.完整性约束

## 1.实体完整性(主键)

### 设置主键

~~~~sql
--方法一：
A CHAR(4),PRIMRY KEY

--方法二：
ALTER TABLE t
	add PRIMARY KEY(A);
~~~~

一个表里面只能有一个主键

### **删除主键**

~~~sql
ALTER TABLE t
	DROP PRIMARY KEY;
~~~



## 2.参照完整性(外键)

### 创建外键

~~~sql
--方法一：
constraint 外键名 foreign key(A) references tmain(A) [ON update/delete cascade]

--方法二：
alter table t2
	add constraint 外键名
	foreign key(A) references tmain(A);
~~~

<u>ON update/delete cascade：级联更新或删除</u>

### **删除外键**

~~~sql
ALTER TABLE t
	DROP constraint 外键名;
~~~



## 3.用户自定义完整性

~~~sql
check(条件)
~~~



## 4.自增约束

~~~sql
auto_increment
~~~

- 一个表只能有一个字段使用AUTO_INCREMENT约束，且该字段必须为主键的一部分。
- 默认情况下，自增字段的初始值为1，每新增一条记录，字段值自动加1



## 5.唯一约束

**值唯一，可有一个且仅有一个空值**

~~~sql
[CONSTRAINT 约束名] UNIQUE(字段名)
~~~



## 6.默认约束

~~~sql
字段定义 default(默认值)
~~~



# 6.SQL语句

## 1.变量

### 全局变量

全局变量是MySQL系统提供并赋值的变量

~~~sql
--查看MySQL版本：
select @@version;
~~~



### 在存储过程、函数或触发器的 `BEGIN ... END` 内声明和使用的变量：

~~~sql
declare A 变量类型; --declare声明的局部变量，变量名前不能加@
set A = 值;
~~~



## 2.存储过程

创建存储过程：

~~~sql
DELIMITER @@
CREATE procedure add(IN num1 INT, IN num2 INT, OUT sum INT)
BEGIN
    set result = num1 + num2;
END @@
~~~

调用：

~~~sql
CALL add_numbers(10, 20, @result);
SELECT @result;  -- 输出 30
~~~

删除存储过程:

```sql
DROP procedure IF exists 存储过程名;
```



## 3.存储函数

创建存储函数：

~~~sql
DELIMITER @@
CREATE function add(IN num1 INT, IN num2 INT, OUT sum INT)
	returns INT
BEGIN
    return num1 + num2;
END @@
~~~

调用：

```sql
SELECT add(10, 20);  -- 结果是 30
```



## 4.判断语句

### if判断语句

~~~sql
if 条件1 then
	...
elseif 条件2 then
		...
	else
		...
end if;
~~~

### case判断语句

~~~sql
--方法一：
case 表达式
	when 值1 then ...
	when 值2 then ...
	when 值3 then ...
end;

--方法二：
case 值
	when 表达式1 then ...
	when 表达式2 then ...
	when 表达式3 then ...
end;
~~~



## 5.循环语句

### loop

~~~sql
标签：LOOP
 ...
    IF 条件 THEN
    	LEAVE 标签;
	END IF;
END LOOP;
~~~

### while循环

~~~sql
WHILE 条件 DO
	...
END WHILE;
~~~

### repeat循环

~~~sql
repeat
    ...
    until 条件
END REPEAT;
~~~



## 6.游标

### 声明游标：

~~~sql
declare 游标名 cursor 
	for select...
~~~

### 打开游标：

~~~sql
open 游标名;
~~~

### 提取数据：

~~~sql
fetch 游标名 into 变量...;
~~~

### 关闭游标：

~~~sql
close 游标名;
~~~

**例子：**

~~~sql
delimiter @@
create procedure stu_proc()
begin
    declare v_ename char(8);
    declare v_address varchar(50);
    declare s_cursor cursor
        for select 姓名,家庭住址 from student_info
        where 学号='0001';
    open s_cursor;
        fetch s_cursor into v_ename, v_address;
    close s_cursor;
    select v_ename, v_address;
end@@

call stu_proc(); 
~~~



# 7.触发器

## 1.创建触发器语法

```sql
create trigger 触发器名
AFTER|BEFORE INSERT|UPDATE|DELETE
ON 表名
for each row
BEGIN
    -- SQL 操作
END;
```



## 2.`NEW` 和 `OLD` 关键字

- 在 `BEFORE` 或 `AFTER INSERT` 中，使用 `NEW.字段名` 访问插入的数据
- 在 `UPDATE` 中：
  - `OLD.字段名` 是更新前的值
  - `NEW.字段名` 是更新后的值
- 在 `DELETE` 中，使用 `OLD.字段名` 访问被删除的数据



# 8.事务

## 1.启动事物

~~~sql
start transaction;/ begin work;
~~~

## 2.提交事务

~~~sql
commit;
~~~

## 3.回滚事务

~~~sql
rollback;
~~~





# 9.权限

## 1.授权

授权表：

~~~sql
grant 操作 on table 数据库名.表名 to 用户;
~~~

授权存储过程/函数：

~~~sql
grant 操作 on procedure/function 数据库名.存储过程名 to 用户;
~~~

授权数据库：

~~~sql
grant 操作 on 数据库名.* to 用户;
~~~



## 2.撤权

撤销指定数据库权限：

~~~sql
revoke 操作 on 数据库名.* from 用户;
~~~

撤销所有权限：

~~~sql
revoke ALL privileges on *.* from st_01@localhost;
~~~





# 10.用户管理

## 1.创建用户

~~~~sql
create user 用户 identified by '密码';
~~~~



## 2.修改用户密码

~~~sql
set password for 用户 = '新密码';
~~~



## 3.修改用户名

~~~sql
rename user 旧用户名 to 新用户名;
~~~



## 4.删除用户

~~~sql
drop user 用户;
~~~



# 11.数据库备份

## 1.备份类型

- **完整备份**
- **差异备份：每次对原来的数据就行差异部分的备份**
- **增量备份：每次对上一次备份的差异部分进行备份**



## 2.备份单个数据库

~~~sql
mysqldump -u用户名 -p密码 -h 主机名 数据库名 > /备份文件名.sql
~~~



## 3.备份单个/多个表

~~~sql
mysqldump -u用户名 -p密码 -h 主机名 数据库名 表名1 表名2 表名3 > /备份文件名.sql
~~~



## 4.恢复备份的表

~~~
mysqldump -u用户名 -p密码 -h 主机名 数据库名 < /备份文件名.sql
~~~



## 5.备份单个数据库

~~~sql
mysqldump -u用户名 -p密码 -h 主机名 数据库名 > /备份文件名.sql
~~~



## 6.备份多个数据库

~~~sql
mysqldump -u用户名 -p密码 -h 主机名 --database 数据库1 数据库2 > /备份文件名.sql
~~~



## 7.备份所有数据库

~~~sql
mysqldump -u用户名 -p密码 -h 主机名 --all > /备份文件名.sql
~~~



## 8.恢复备份的数据库

~~~sql
mysqldump -u用户名 -p密码 -h 主机名 要回到的数据名 < /备份文件名.sql
~~~

