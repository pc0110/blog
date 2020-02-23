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

# order by 默认是升序
select * from student order by sage desc;//desc是从高到低
select * from student order by sage asc;//从低到高
//order by 子句冲可以放单个字段、多个字段、表达式、函数、别名、

# 按年薪的高低显示员工的信息和年薪(按别名排序) 
select *,salary*12*(1+ifnull(commission_pct,0)) 年薪 from employees order by 年薪 desc;
//判断如果没有奖金率

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


# union 字符串 就是将两个查询语句进行组合
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

# 事务 处理数据量大，复杂
1.只有Mysql中只有Innodb支持事务
2.事务处理的数据库要么全部执行，要么全部不执行
3.事务用来管理insert,update,delete语句
它满足四个条件：
    原子性：一个事务中的所有的操作，要么全部完成，要么全部不完成
    一致性：在事务的开始和事务的结束以后，数据库的完整性没有被破坏
    隔离性：数据库允许多个并发事务同时对其数据库进行读写和修改的能力，隔离性可以防止多个事务并发执行时由于交叉执行而导致数据的比一至，事务隔离分为不同级别，包括：未提交,读提交，可重复读和串行化
    持久性：事务处理结束后，对数据的修改就是永久的，即使系统故障也不会丢失

## 事务的实现的两种方式：
    1.使用begin,rollback,commit来实现,分别是开始，回滚，四五确认
    2.使用set来改变mysql的自动提交模式
    set autocommit=0禁止自动提交
    set autocommit=1开启自动提交

# 索引
创建索引
create index 索引名称 on 表名(列名(length));
### 创建唯一索引
create unique index indexname on student(name(length));

# 临时表 MySQL 临时表在我们需要保存一些临时数据时是非常有用的。临时表只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。

# 复制表 如果我们需要完全的复制MySQL的数据表，包括表的结构，索引，默认值等。 如果仅仅使用CREATE TABLE ... SELECT 命令，是无法实现的。

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

# 分页查询
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

# 练习：查询

``` mysql
1.
select(
    select salary
        from emplory
        limit 1 offset 1
    ) as secondhighestsalary;



```

# 练习：删除

```mysql
1.
    DELETE p1 
    FROM Person p1,Person p2
    WHERE
        p1.Email = p2.Email AND p1.Id > p2.Id
    ;
2.    
   
```



# 列转行

```mysq
SELECT year,
    MAX(CASE month WHEN '1' THEN amount ELSE 0 END ) m1,
    MAX(CASE month WHEN '2' THEN amount ELSE 0 END ) m2,
    MAX(CASE month WHEN '3' THEN amount ELSE 0 END ) m3,
		MAX(CASE month WHEN '4' THEN amount ELSE 0 END ) m4
FROM test
GROUP BY year;
```

