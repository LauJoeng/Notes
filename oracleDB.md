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
