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

撤销授权:revoke  SELECT on "SCOTT"."EMPLOYEES" from "TEST"; 

