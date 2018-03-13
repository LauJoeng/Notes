**Oracle数据库**

- 使用时需要将Oracle的一个Listener和一个OracleServiceORCL服务开启。并且最好把他们设置为手动开启，用完后手动关闭。

- 可以在system用户下创建新的用户，并通过
 grant connect,resource,unlimited tablespace to username;
语句对用户授权

- 使用OEM(Enterprise Manager Database Express)

```
Copyright (c) 1982, 2014, Oracle. All rights reserved.

请输入用户名: system 
输入口令: 
上次成功登录时间: 星期五 1月 09 2015 18:42:49 +08:00

连接到: 
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production 
With the Partitioning, OLAP, Advanced Analytics and Real Application Testing opt 
ions 
SQL> show parameter dispatchers; 
NAME TYPE VALUE

dispatchers string (PROTOCOL=TCP) (SERVICE=oracle 
12cXDB) 
max_dispatchers integer 
SQL> exec dbms_xdb_config.sethttpport(8088); 
PL/SQL 过程已成功完成。 
上面的过程把端口号改为8088然后访问http://localhost:8088/em就可以了
```

- 使用OEM管理授权

在对象权限中，可以为某用户授予访问其他用户下表的权限。
比如为test用户授予scott用户下employees的select权限。点击安全，选择用户，选择test用户。进入后点击对象权限，点击授权。其中方案为用户名，对象类型选择table，然后对象名即为表名，点击下一步即可看到一些操作，有select，delete，insert等操作。

sql语句即为：grant  SELECT  on  “SCOTT”."EMPLOYEES" to "TEST"; 

撤销select授权:revoke  SELECT on "SCOTT"."EMPLOYEES" from "TEST"; 

其他权限也需要授予，如创建表的权限，sql语句:grant CREATE ANY TABLE to "TEST";

可以为用户表空间设置限额大小，sql语句:alter user "TEST" quta 10M on "USERS";	TEST 是用户名，10M是大小，单位从小到大分别是K,M,G,T,E等，USERS是表空间名。

可以把角色赋给一个用户，则该用户就相当于这个角色。这样test就有dba的所有权限。比如把dba赋给test，则test就是一个dba。sql语句:grant "DBA" to "TEST";

用户表空间中的system表空间相当于电脑的c盘，所以最好不要把自己创建的表空间使用system表空间

oracleDB中空值不为0，一切与空值进行运算的结果都是空值

查询时可以用 || 连接字符串，如 select last_name || '`s job is' job from employees;

oracleDB函数

to_char函数对日期的转换:to_char(date,'format_model'),注意单引号中大小写敏感。

如:SELECT TO_CHAR(sysdate,‘yyyy-mm-dd hh:mi:ss’) FROM dual;

通用函数

- NVL (expr1, expr2)  将空值转换为一个已知的值
- NVL2 (expr1, expr2, expr3) expr1不为空返回expr2，否则返回expr3
- NULLIF (expr1, expr2) 相等返回null，不等返回expr1
- COALESCE (expr1, expr2, ..., exprn)  COALESCE 与 NVL 相比的优点在于 COALESCE 可以同时处理交替的多个值。

oracle中可以用 (+) 表示外连接，如：
select e.last_name,e.department_id,d.department_name from employees e,departments d where e.department_id = d.department_id(+); (左连接)

select e.last_name,e.department_id,d.department_name from employees e,departments d where e.department_id(+) = d.department_id;(右连接)
<hr>
**视图**:

- 视图是一种虚表
- 视图建立在已有表的基础上，视图赖以建立的这些表称为基表
- 向视图提供数据内容的语句为select语句，可以将视图理解为存储起来的select语句
- 视图是想用户提供数据的另一种表现形式

为什么使用视图

- 控制数据访问
- 简化查询
- 避免重复访问相同数据

授予用户创建视图的权限:grant create view to "SCOTT";（注意双引号里大小写敏感）,或在OEM里面进行操作。

对视图的增删改的操作也会影响基表的数据

修改视图:使用create or replace view 修改 视图

ex:
create or replace view empview2
(id_number,name,sal,department_id)
as select employee_id,first_name || ' ' || last_name,salary,department_id
from employees
where department_id=80;

限制视图操作.

只读视图:
create or replace view empview2
as
select employee_id id,last_name	department_name
from employees e,departments d
where e.department_id = d.department_id
with read only;

使用了组函数的视图属于复杂视图，复杂视图不能使用DML操作

TOP-N分析:分析查询一个列中最大或最小的n个值

rownum是查询出来结果的一个伪序列，即第几个。则可以通过rownum查询出前10个结果

ex:
select rownum employee_id,last_name,salary
from (
			select rownum employee_id,last_name,salary
			from employees
			order by salary desc
)
where rownum <= 10;

上述语句，通过子查询获得salary排序的结果，在通过rownum查询出前10的结果

注意:rownum只能使用<或 <= ,而= ,> , >= 都将不能返回任何数据

查询salary排40-50之间的数据如下:

select rn, employee_id,last_name,salary
from(
      select rownum rn ,employee_id,last_name,salary
      from (
            select rownum employee_id,last_name,salary
            from employees
            order by salary desc
      )
)
where rn > 40 and rn <=50;

将上面查询到的结果再一次作为一个子查询，则上面查询到的rownum就作为子查询的到的表中的一个普通的列，这样就可以间接的对rownum使用 > ,>= 等操作了。
<hr>
**序列**
序列是可供多个用户用来产生唯一数值的数据库对象
- 自动提供唯一的值
- 共享对象
- 主要用于提供主键值（相当于mysql中的auto_increment,这也是主要用途）
- 将序列值装入内存可以提供访问效率

创建序列

create sequence se
			[increment by n]  --每次增长的数值
			[start with n]       --从哪个值开始
			[{maxvalue n | nomaxvalue}]
			[{minvalue n | nominvalue}]
			[{cycle | nocycle}] --是否需要循环
			[{cache n | nocahce}]  -- 是否需要缓存登录
	
**索引**
- 一种独立于表的模式对象，可以存储在与表不同的磁盘或表空间中
- 索引被破坏，不会对表产生影响，只会影响查询的速度
- 索引一旦建立，Oracle系统会对其进行自动维护，而且Oracle管理系统决定何时使用索引，用户不用在查询语句中指定使用哪个索引
- 在删除一个表时，所有基于该表的索引会自动被删除u
- 通过指针加速Oracle服务器的查询速度
- 通过快速定位数据的方法，减少磁盘I/O

在一个或多个列上创建索引
create index idx on table (column[,column]...);

在表employees的列last_name上创建索引
create index on emp_last_name_idx  on employees(last_name);

**同义词**

使用同义词访问相同的对象
- 方便访问其他用户对象
-  缩短对象名字的长度

create [public] synoym name for object;


如:create  synoym e for employees;
select * from e; 
