# SQL复习



建立数据库

```sql
CREATE DATABASE 数据库名;

销毁数据库
DROP DATABASE 数据库名;
```

建立表

```sql
create table Book
(
	book_id varchar(255) not null primary key,
    book_name varchar(255)
    
    段外约束
    PRIMARY KEY(字段,字段,...)
    FOREIGN KEY(字段) REFERENCES 外表(主键)
)

销毁表
DROP TABLE 表名;
```

修改表

```SQL
alter table 表名
ADD 字段名 字段类型
DROP column 字段名
alter column 字段名 字段类型
add constraint 约束名 约束表达式
drop constraint 约束名
```

增加记录

```sql
insert into 表名(字段1,字段2,字段3) values(值1,值2,值3);
```

修改记录

```sql
update 表名 set 字段1=值1,字段2=值2,字段3=值3 
	where 条件表达式
```

删除记录

```sql
delete from 表名 
	where 条件表达式
```

select

```sql
select 字段1,字段2
	FROM 表名,表名
	[JOIN 表名 ON 相等条件表达式]
	[WHERE 条件表达式]
	[GROUP BY 字段]
	[HAVING 条件表达式]
	[ORDER BY 字段]
```

表达式

```sql
算数运算符
+ - * / %
比较运算符
= < <= > >=  BETWEEN  AND
模式匹配运算符
LIKE
逻辑运算符
NOT AND OR
判空运算符
IS //即写作IS NULL 而不是=NULL
```

### 约束

命名主键约束或为多个列定义主键约束

```sql
CONSTRAINT pk_PersonID PRIMARY KEY (Id_p,LastName)
```

外键约束

外键添加：一个表中的FOREIGN KEY 指向另一个表中的PRIMARY KEY

```sql
Id_P int foreign key references Persons(Id_P)

CONSTRAINT fk_PerOrders FOREIGN KEY (Id_P) REFERENCES Persons(Id_P)
```

from子句

```sql
from (子查询) AS 子查询集名
FROM (SELECT Fid,Name FROM food WHERE Price<10) AS cheap_food
```

where子句

```sql
where <子查询条件表达式>
<子查询条件表达式>可以形如
EXSISTS(<select 语句>)
columnName[not]in(<select 语句>)
columnName>=[any|some|all](<select 语句>)
```

EXSITS子查询

```sql
WHERE EXISTS (子查询)

where [not]exists (select * from food where city='北京')
```

IN子查询

```sql
where columnName [not] IN(子查询)
where fid not in (select fid from food where City='北京')
```

TOP修饰符

```sql
SELECT TOP M * from
```

DISTINCT修饰符

```sql
SELECT [DISTINGCT|ALL] * from
默认为all，不忽略重复行
制定distinct后即可忽略重复行
```

UNION子句

```sql
<select 语句>
UNION
<select 语句>
```

子查询 联表查询

```sql
子查询一般更加强大但效率较低
联表查询:
select 学号
from 选课表 join 课程表
on 选课表.课程号=课程表.课程号
where 课程名='数据库'

子查询:
select 学号
from 选课表
where 课程号=(
	select 课程号 from 课程表 where 课程名='数据库'
)
```

JOIN表连接

```sql
select Persons.LastName,Persons.FirstName,Orders.OrderNo
From Persons join Orders On persons.Id_p=Orders.Id_p
效果等同于
SELECT Persons.LastName,Persons.FirstName,Orders.OrderNo
From Persons,Orders
where Persons.Id_p=Orders.Id_P
```

GROUP BY：用于结合聚合函数，根据一个或多个列对结果集进行分组。

聚合函数：AVG(),COUNT(),MAX(),FIRST(),LAST(),SUM()

HAVING：where关键字无法与聚合函数一起使用你，因此用Having进行分组后聚合函数的筛选

ORDER BY：默认按照升序(ASC)对记录进行排序，降序(DESC)需手动指定，可用于多关键字排序

```sql
select Customer,SUM(OrderPrice)
form Orders,Customers
where ...
GROUP By Customer
Having sum(OrderPrice)>..
Order By Age DESC,Customer ASC;
```

视图VIEW

```sql
create view 视图名 as
查询语句
with check option
```

索引

```sql
create unique|cluster index 索引名 on 表名
(列名[次序],列名[次序])
```

check约束

```sql
create table sells(
	beer char check(),
    price real check(),
    check(beer='' or price...)
)
```

foreign key 约束 

```sql
CREATE TABLE Beers (
	name	CHAR(20) PRIMARY KEY,
	manf	CHAR(20) );

CREATE TABLE Sells (
	bar	CHAR(20),
	beer	CHAR(20) REFERENCES Beers(name),
	price	REAL );
CREATE TABLE Sells (
	bar	CHAR(20),
	beer	CHAR(20),
	price	REAL,
	FOREIGN KEY(beer) REFERENCES 	Beers(name));
CREATE TABLE Sells (
	bar	CHAR(20),
	beer	CHAR(20),
	price	REAL,
	FOREIGN KEY(beer)
		REFERENCES Beers(name)
		ON DELETE SET NULL
		ON UPDATE CASCADE );

```

assertions

```sql
CREATE ASSERTION <name>
			CHECK ( <condition> );

```

trigger

```sql
CREATE TRIGGER ViewTrig
	INSTEAD OF INSERT ON Synergy
	REFERENCING NEW ROW AS n
	FOR EACH ROW
	BEGIN
		INSERT INTO LIKES VALUES(n.drinker, n.beer);
		INSERT INTO SELLS(bar, beer) VALUES(n.bar, n.beer);
		INSERT INTO FREQUENTS VALUES(n.drinker, n.bar);
	END;

```



## model

现实世界

概念模型 conceptual model

逻辑模型 logical model

物理模型 physical model

计算机世界



E/R relationship diagrams

![1560855426620](D:\source_code\note\sql\SQL复习.md)



alternative notation

![1560855518173](D:\source_code\note\sql\1560855518173.png)



## 范式

2NF 消除了数据冗余，避免数据更新所造成的数据不一致性的问题。

3NF数据冗余度降低，不存在插入异常，不存在删除异常，不存在更新异常



## 先进数据库技术

关系模型

缺点： 有限的数据类型

不能清晰表达复杂对象和对象之间的关系

缺少对象身份标识

O-R映射

将对象数据映射到关系数据库的表中

