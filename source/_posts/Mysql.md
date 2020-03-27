---
title: Mysql
date: 2019-11-03 19:30:31
tags: 
- Mysql
- 数据库
cover:  "https://ss1.bdstatic.com/70cFuXSh_Q1YnxGkpoWK1HF6hhy/it/u=4085049135,313268802&fm=26&gp=0.jpg"
---

# 存储引擎：Innodb myisam,memory,blackhole
MySQL中的数据用各种不同的技术存储在文件（或者内存）中。这些技术中的每一种技术都使用不同的存储机制、索引技巧、锁定水平并且最终提供不同的功能和能力。
通过选择不同的技术，你能够获得额外的速度或者功能，从而改善你的应用的整体功能。

**MyISAM**：拥有较高的插入，查询速度，但不支持事务，不支持外键。
**InnoDB**：支持事务，支持外键，支持行级锁定，性能较低。
**blackhole**:创建立马删除
**memory**：只存在于内存中

# 创建表的完整性约束
create  table 表名(
    字段名1 类型[(宽度) 约束条件],
    字段名2 类型[(宽度) 约束条件],
    字段名3 类型[(宽度) 约束条件]
);

注意：字段名和字段类型是必须的，最后一个字段不能加逗号

宽度：char只能存一个字符

alter table 表名 modify 字段名 字段类型 约束条件 
//自增条件
auto_increment

# 数据类型
```bash

<数值类型>
smallint  2字节
int       4字节
bigint    8字节   

float     4字节
double    8字节

<日期和时间类型>
date      3字节
time      3字节
year      1字节
datetime  8字节
timestamp 4字节

<字符串类型>
char    0-255
varchar 0-65535
text    0-65535

```
# 查询版本
select version();
# 别名
select 100*23 as 别名；
select 100*23 别名;

# distinct 去重
select distinct 字段名 from 表名；

# unsigned

# concat 
select 'qwe'+123; 结果是123 不能拼接字符串
select null+123; 结果是null 任何值和NULL都是null
select concat(字段名。字段名) as 别名 from 表名；

# ifnull(字段名,返回值)
当使用拼接语句时，因为存在可能有的属性为空的情况下
select concat(ifnull(saddress,0),sname) as out_put from student;

# 模糊查询Like 只是通配符 不能正则
通配符：%匹配任意字符 _匹配单个字符
查询第名字第二位为a和第四位为s的人的名字
select * from student where sname like '_a_s';
escape//转义
select * from student where sname like '_$_' escape '$'; //通过指定转义字符

# 正则表达式 rlike 可以是正则表达式 需要
查找name字段中以'st'为开头的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP '^st';

查找name字段中以'ok'为结尾的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP 'ok$';

查找name字段中包含'mar'字符串的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP 'mar';

查找name字段中以元音字符开头或以'ok'字符串结尾的所有数据：
mysql> SELECT name FROM person_tbl WHERE name REGEXP '^[aeiou]|ok$';

# between and 包含临界值
select * from student where sno between 100 and 120;

# in 
select *sname* form student where  sname in('zs','ls');

# is null / is not null
select * from student where monmery is null;
select * from student where monmery is not null;

# <=> 安全等于
select * from student where sname <=> 'qwe';

# <>  != 不等于
select * from stdetn where sage <> 21;

# order by 

默认是升序

select * from student order by sage desc;//desc是从高到低
select * from student order by sage asc;//从低到高
//order by 子句冲可以放单个字段、多个字段、表达式、函数、别名、



# 常见函数
//调高代码的重用性，隐藏了实现细节
函数的调用：select 函数名(参数列表) [from表]：
##### 1.字符函数length //获取参数值的字节个数
select length('张三');
##### 2.concat拼接函数
select concat(last_name,'_',first_name) 姓名 from student;
##### 3.upper 、lower
select concat(supper(last_name),lower(first_name)) 姓名 from student；
##### 4.substr、substring//截取字符串
select substr('大撒大撒大撒法'，4) out_put;//mysql的索引是从1开始的
select concat(upper(substr(last_name,1,1)),'_',lower(substr(first_name,2))) out_put;
##### 5.instr返回字串第一次出现的索引
select instr('qweqwe','qwe') as out_put;
##### 6.trim
select length(trim('张翠山')) as out_put;
##### 7.lpad 用指定的字符实现左填充指定长度
select lpad('殷素素',10,'*') as out_put;
##### 8.rpad 用指定的字符实现左填充指定长度
select rpad('殷素素',10,'*') as out_put;
##### 9.replace 替换
select replace('我爱你','我','你') as out_put;

## 数学函数
##### round四舍五入
select round(1.55);

#### ceil向上取整
select ceil(1.00)

#### floor向下取整
select floot(-9.99);

#### truncate 截断
select truncate(2.54984,1);

#### mod取余
select mod(6,3);

## 日期函数
#### now 返回当前系统日期+时间
#### curdate
#### curtime
#### year
#### str_to_date('1998-9-20','%Y-%d-%c')
#### selectdatediff(now(),'1999-05-22') //可以查询日期的差值
#### timestampdiff(year/day/hour/second,起始值，结束值)计算从出生日期到现在的年数

## 流程控制函数
#### if(条件,trur,false)

#### case： switch case
case 表达式
when 常量1 then 要显示的值1或语句1；
when 常量1 then 要显示的值2或语句2；
else 要显示的值
end
## 分组函数 sum avg min max count 

# 分组查询
select 查询的函数或值，要分组的字段(可以放多个字段)
from表名
【where 可以加条件】
group by 字段
having 根据筛选条件在进行筛选
【order by 字段 desc】；
分组函数做条件肯定是在having中
能用分组前筛，就放在分组前面

# 连接查询
//多表查询，当需要查询的字段涉及到多个表时，就需要连接查询
笛卡尔乘积现象：表1 有m行,表2有n行，查询的结果时m*n行
select column1,culumn2  
from table1,table2
where table1.id=table2.id
and 条件
group by 字段

# union 字符串 

就是将两个查询语句进行组合

select address,name from student
    where address='qwe'
     union 	distinct
     select address,name from teacher
     where address='qwe'
     order by address;

# 连接：
```
    inner join :获取两个表中字段字段匹配关系的记录
    left join：获取左表左右记录，即使有表没有对应匹配的记录
    right join：获取右表所有的记录，即使左表没有对应的记录

    select 列名
    from 主表
    left outer join 副表
    on 条件 一般是相同的id值
    where 条件
```
# 子查询/内查询，外部的查询语句，称为主查询或外查询
分类：按子查询出现的位置：
1.select后面：仅仅支持标量子查询（结果集只有一行一列）
2.from后面：支持表子查询（结果集一般分为多行多列）
3.where或having后面：标量子查询（单行）、列子查询（多行）、行子查询（一行多列）、列子查询（一列多行）
4.exists后面（相关子查询）：表子查询（多行）
//一般使用标量子查询的较多

# 事务

 处理数据量大，复杂

1.只有Mysql中只有Innodb支持事务
2.事务处理的数据库要么全部执行，要么全部不执行
3.事务用来管理insert,update,delete语句
它满足四个条件：
    原子性：一个事务中的所有的操作，要么全部完成，要么全部不完成
    一致性：在事务的开始和事务的结束以后，数据库的完整性没有被破坏
    隔离性：数据库允许多个并发事务同时对其数据库进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的比一至，事务隔离分为不同级别，包括：未提交,读提交，可重复读和串行化
    持久性：事务处理结束后，对数据的修改就是永久的，即使系统故障也不会丢失

## 事务的实现的两种方式：
    1.使用begin,rollback,commit来实现,分别是开始，回滚，确认
    2.使用set来改变mysql的自动提交模式
    set autocommit=0禁止自动提交
    set autocommit=1开启自动提交

![](E:\blog\public\images\事务级别.jpg)

# 锁

锁是网络数据库中的一个非常重要的概念，当多个用户同时对数据库并发操作时，会带来数据不一致的问题，所以，锁主要用于多用户环境下保证数据库完整性和一致性。

帮助理解：以商场的试衣间为例，每个试衣间都可供多个消费者使用，因此，可能出现多个消费者同时需要使用试衣间试衣服。为了避免冲突，试衣间装了锁，某一个试衣服的人在试衣间里把锁锁住了，其他顾客就不能从外面打开了，只能等待里面的顾客试完衣服，从里面把锁打开，外面的人才能进去。

数据库锁出现的目的：处理并发问题

并发控制的主要采用的技术手段：乐观锁、悲观锁和时间戳。

## 锁分类
从数据库系统角度分为三种：排他锁、共享锁、更新锁。
从程序员角度分为两种：一种是悲观锁，一种乐观锁。

### 悲观锁（Pessimistic Lock）
顾名思义，很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人拿这个数据就会block（阻塞），直到它拿锁。

悲观锁（Pessimistic Lock）：正如其名，具有强烈的独占和排他特性。它指的是对数据被外界（包括本系统当前的其他事务，以及来自外部系统的事务处理）修改持保守态度，因此，在整个数据处理过程中，将数据处于锁定状态。悲观锁的实现，往往依靠数据库提供的锁机制（也只有数据库层提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统中实现了加锁机制，也无法保证外部系统不会修改数据）。

传统的关系数据库里用到了很多这种锁机制，比如行锁、表锁、读锁、写锁等，都是在操作之前先上锁。

悲观锁按使用性质划分
#### 共享锁（Share Lock）
S锁，也叫读锁，用于所有的只读数据操作。共享锁是非独占的，允许多个并发事务读取其锁定的资源。
性质

1. 多个事务可封锁同一个共享页；
2. 任何事务都不能修改该页；
3. 通常是该页被读取完毕，S锁立即被释放。

在SQL Server中，默认情况下，数据被读取后，立即释放共享锁。
例如，执行查询语句“SELECT * FROM my_table”时，首先锁定第一页，读取之后，释放对第一页的锁定，然后锁定第二页。这样，就允许在读操作过程中，修改未被锁定的第一页。
例如，语句“SELECT * FROM my_table HOLDLOCK”就要求在整个查询过程中，保持对表的锁定，直到查询完成才释放锁定。

#### 排他锁（Exclusive Lock）
X锁，也叫写锁，表示对数据进行写操作。如果一个事务对对象加了排他锁，其他事务就不能再给它加任何锁了。（某个顾客把试衣间从里面反锁了，其他顾客想要使用这个试衣间，就只有等待锁从里面打开了。）
性质

1. 仅允许一个事务封锁此页；
2. 其他任何事务必须等到X锁被释放才能对该页进行访问；
3. X锁一直到事务结束才能被释放。

产生排他锁的SQL语句如下：select * from ad_plan for update;

#### 更新锁
U锁，在修改操作的初始化阶段用来锁定可能要被修改的资源，这样可以避免使用共享锁造成的死锁现象。

因为当使用共享锁时，修改数据的操作分为两步：
1. 首先获得一个共享锁，读取数据，
2. 然后将共享锁升级为排他锁，再执行修改操作。
这样如果有两个或多个事务同时对一个事务申请了共享锁，在修改数据时，这些事务都要将共享锁升级为排他锁。这时，这些事务都不会释放共享锁，而是一直等待对方释放，这样就造成了死锁。
如果一个数据在修改前直接申请更新锁，在数据修改时再升级为排他锁，就可以避免死锁。

性质
1. 用来预定要对此页施加X锁，它允许其他事务读，但不允许再施加U锁或X锁；
2. 当被读取的页要被更新时，则升级为X锁；
3. U锁一直到事务结束时才能被释放。

悲观锁按作用范围划分为：行锁、表锁。（这部分摘自https://www.jianshu.com/p/eb41df600775 ）
#### 行锁
锁的作用范围是行级别。

#### 表锁
锁的作用范围是整张表。

数据库能够确定那些行需要锁的情况下使用行锁，如果不知道会影响哪些行的时候就会使用表锁。

举个例子，一个用户表user，有主键id和用户生日birthday。
当你使用update … where id=?这样的语句时，数据库明确知道会影响哪一行，它就会使用行锁；
当你使用update … where birthday=?这样的的语句时，因为事先不知道会影响哪些行就可能会使用表锁。

### 乐观锁（Optimistic Lock）
顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以，不会上锁。但是在更新的时候会判断一下在此期间别人有没有更新这个数据，可以使用版本号等机制。

乐观锁（ Optimistic Locking ）： 相对悲观锁而言，乐观锁机制采取了更加宽松的加锁机制。
悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。但随之而来的就是数据库性能的大量开销，特别是对长事务而言，这样的开销往往无法承受。而乐观锁机制在一定程度上解决了这个问题。
乐观锁，大多是基于数据版本（ Version ）记录机制实现。
数据版本：为数据增加一个版本标识，在基于数据库表的版本解决方案中，一般是通过为数据库表增加一个 “version” 字段来实现。读取出数据时，将此版本号一同读出，之后更新时，对此版本号加一。此时，将提交数据的版本数据与数据库表对应记录的当前版本信息进行比对，如果提交的数据版本号大于数据库表当前版本号，则予以更新，否则认为是过期数据。

乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库如果提供类似于write_condition机制的其实都是提供的乐观锁。

乐观锁的实现方式（这部分摘自https://www.jianshu.com/p/eb41df600775 ）
#### 版本号（version）
版本号（记为version）：就是给数据增加一个版本标识，在数据库上就是表中增加一个version字段，每次更新把这个字段加1，读取数据的时候把version读出来，更新的时候比较version，如果还是开始读取的version就可以更新了，如果现在的version比老的version大，说明有其他事务更新了该数据，并增加了版本号，这时候得到一个无法更新的通知，用户自行根据这个通知来决定怎么处理，比如重新开始一遍。这里的关键是判断version和更新两个动作需要作为一个原子单元执行，否则在你判断可以更新以后正式更新之前有别的事务修改了version，这个时候你再去更新就可能会覆盖前一个事务做的更新，造成第二类丢失更新，所以你可以使用update … where … and version=”old version”这样的语句，根据返回结果是0还是非0来得到通知，如果是0说明更新没有成功，因为version被改了，如果返回非0说明更新成功。

#### 时间戳（使用数据库服务器的时间戳）

时间戳（timestamp）：和版本号基本一样，只是通过时间戳来判断而已，注意时间戳要使用数据库服务器的时间戳不能是业务系统的时间。

待更新字段

待更新字段：和版本号方式相似，只是不增加额外字段，直接使用有效数据字段做版本控制信息，因为有时候我们可能无法改变旧系统的数据库表结构。假设有个待更新字段叫count,先去读取这个count,更新的时候去比较数据库中count的值是不是我期望的值（即开始读的值），如果是就把我修改的count的值更新到该字段，否则更新失败。java的基本类型的原子类型对象如AtomicInteger就是这种思想。

所有字段
所有字段：和待更新字段类似，只是使用所有字段做版本控制信息，只有所有字段都没变化才会执行更新。
乐观锁几种方式的区别（这部分摘自https://www.jianshu.com/p/eb41df600775 ）
新系统设计可以使用version方式和timestamp方式，需要增加字段，应用范围是整条数据，不论那个字段修改都会更新version,也就是说两个事务更新同一条记录的两个不相关字段也是互斥的，不能同步进行。旧系统不能修改数据库表结构的时候使用数据字段作为版本控制信息，不需要新增字段，待更新字段方式只要其他事务修改的字段和当前事务修改的字段没有重叠就可以同步进行，并发性更高。
并发控制会造成两种锁（这部分摘自https://www.cnblogs.com/ismallboy/p/5574006.html）
活锁
#### 死锁
并发控制会造成活锁和死锁，就像操作系统那样，会因为互相等待而导致。

#### 活锁
定义：指的是T1封锁了数据R，T2同时也请求封锁数据R，T3也请求封锁数据R，当T1释放了锁之后，T3会锁住R，T4也请求封锁R，则T2就会一直等待下去。
解决方法：采用“先来先服务”策略可以避免。

死锁
定义：就是我等你，你又等我，双方就会一直等待下去。比如：T1封锁了数据R1，正请求对R2封锁，而T2封住了R2,正请求封锁R1，这样就会导致死锁，死锁这种没有完全解决的方法，只能尽量预防。
预防方法：
1. 一次封锁法，指的是一次性把所需要的数据全部封锁住，但是这样会扩大了封锁的范围，降低系统的并发度；
2. 顺序封锁法，指的是事先对数据对象指定一个封锁顺序，要对数据进行封锁，只能按照规定的顺序来封锁，但是这个一般不大可能的。

系统判定死锁的方法：

超时法：如果某个事物的等待时间超过指定时限，则判定为出现死锁；
等待图法：如果事务等待图中出现了回路，则判断出现了死锁。
对于解决死锁的方法，只能是撤销一个处理死锁代价最小的事务，释放此事务持有的所有锁，同时对撤销的事务所执行的数据修改操作必须加以恢复。



# 索引

创建索引
create index 索引名称 on 表名(列名(length));
### 创建唯一索引
create unique index indexname on student(name(length));

### 临时表 MySQL 临时表在我们需要保存一些临时数据时是非常有用的。临时表只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。

### 复制表 如果我们需要完全的复制MySQL的数据表，包括表的结构，索引，默认值等。 如果仅仅使用CREATE TABLE ... SELECT 命令，是无法实现的。

# 序列 
Mysql的序列化是一组数组
最简单的方法就是用auto_increment来定义一个列让其实现自增的方法

# SQL注入
所谓的SQL注入就是把SQL 命令插入到web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令
我们需要注意以下几点：
1.永远不要新人用户的输入，对用户的输入进行校验，可以通过正则表达式，或者是限制长度，对单引号和双“-”进行转换等
2.永远不要使用动态拼装SQL，可以使用参数化的sql或者直接使用存储过程进行数据查询存取
3.永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。
4.不要把机密信息直接存放，加密或者hash掉密码和敏感的信息。
5.应用的异常信息应该给出尽可能少的提示，最好使用自定义的错误信息对原始错误信息进行包装
6.sql注入的检测方法一般采取辅助软件或网站平台来检测，软件一般采用sql注入检测工具jsky，网站平台就有亿思网站安全平台检测工具。MDCSOFT SCAN等。采用MDCSOFT-IPS可以有效的防御SQL注入，XSS攻击等。   

# 练习



## 查询

语法：
```mysql
    select 查询列表
    from 表
    [join type join 表2
    on 连接条件
    where 筛选条件
    group by 分组字段
    having 分组后的筛选 
    order by 排序的字段]
    limit offset,size;
    offset：要显示条目的起始索引（起始索引从0开始）
    size：要显示的条目个数
```

## 查询

``` mysql
1.
select(
    select salary
        from emplory
        limit 1 offset 1
    ) as secondhighestsalary;



```

## 删除

```mysql
1.
    DELETE p1 
    FROM Person p1,Person p2
    WHERE
        p1.Email = p2.Email AND p1.Id > p2.Id
    ;
2.    
   
```

## 列转行

```mysq
SELECT year,
    MAX(CASE month WHEN '1' THEN amount ELSE 0 END ) m1,
    MAX(CASE month WHEN '2' THEN amount ELSE 0 END ) m2,
    MAX(CASE month WHEN '3' THEN amount ELSE 0 END ) m3,
		MAX(CASE month WHEN '4' THEN amount ELSE 0 END ) m4
FROM test
GROUP BY year;
```

## 按年薪的高低显示员工的信息和年薪(按别名排序) 

select *,salary*12*(1+ifnull(commission_pct,0)) 年薪 from employees order by 年薪 desc;
//判断如果没有奖金率