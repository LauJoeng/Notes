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

**Oracle其他操作:**

创建用户

create user username identified by password；

授予用户权限

grant privilege to user;

为用户分配表空间

alter user yang quota unlimited on users;（user为用户名，users为表空间）

创建角色并赋予权限

创建角色:create role manager;

为角色赋予权限:grant create table,create view to manager;

将角色赋予用户:grant manager to dehaan,kochar;

分配对象权限

grant select on employees to sue,rich;为用户分配employees表的查询权限.

with grant option和public关键字

with grant option:（使用户同样具有分配权限的权利）

grant select on employees to sue,rich with grantoption ;
     
 向数据库中所有用户分配权限
grant select on alice.departments to public ;

收回对象权限:

revoke privilege  on object from user [casecade constraints ];

 **PL/SQL**
先开启:set serveroutput on

PL/SQL程序由声明部分、执行部分、异常处理部分三部分组成。

PL/SQL块的结构如下

declare
/*声明部分，在此声明PL/SQL用到的变量，类型及游标，以及局部变量的存储过程和函数*/
begin
  /*执行部分:过程及SQL语句，即程序的主要部分*/
exception
 /*执行异常部分，错误处理*/
end

其中执行部分是必须的


可以用%type，%rowtype定义变量

如:

```
declare
  v_sal employees.salary%type;
  v_email employees.email%type;
  v_hiredateemployees.hire_date%type;
begin
        select salary,email,hire_date into v_sal,v_email,v_hiredate from employees where employee_id=100;
        dbms_output.put_line(v_sal||','||v_email||','||v_hiredate);
end;
```
用%type定义变量的好处是
- 所引用的数据库列的数据类型不必知道
- 所引用的数据库列的数据类型可以实时改变

记录类型:记录类型是把逻辑相关的数据单元作为一个单元存储起来，称作PL/SQL record的域(FIELD),其作用是存放互不相同但逻辑相关的信息，有点像Java中的类

声明一个记录类型:type ...
ex:

```
declare
--定义一个记录类型
  type emp_record is record(
  v_sal employees.salary%type,
  v_email employees.email%type,
  v_hire_date employees.hire_date%type
  );
--定义一个记录类型的成员变量
    v_emp_record emp_record;
begin
        select salary,email,hire_date into v_emp_record from employees where employee_id=100;
        dbms_output.put_line(v_emp_record.v_sal||','||v_emp_record.v_email||','||v_emp_record.v_hire_date);
end;
```
或
```
declare
/*
  type emp_record is record(
  v_sal number(8,2):=0,
  v_emp_id number(10)
  );
  */
  --定义一个记录类型的成员变量，该变量对应表的结构
  v_emp_record employees%rowtype;
begin
  select * into v_emp_record from employees  where employee_id=123;
  dbms_output.put_line('employee_id:'||v_emp_record.employee_id||','||'salary:'||v_emp_record.salary);
--exception
  
end;
```

l流程控制

条件判断(两种)
方式1：if ... than elseif than ... else ... end if;

ex:

```
declare
  v_sal employees.salary%type;
begin
  select salary into v_sal from employees where employee_id=150;
  
  if v_sal >= 10000 then dbms_output.put_line('salary >= 10000');
  elsif v_sal >= 5000 then dbms_output.put_line('5000 <= salary <= 10000');
  else dbms_output.put_line('salary < 5000');
  end if;
end;
```

方式2:   case ... when ... then ... end;

ed

```
declare
  v_sal employees.salary%type;
  v_temp varchar2(30);
begin
  select salary into v_sal from employees where employee_id=150;
  v_temp :=
  case trunc(v_sal/5000) when 0 then 'salary < 5000'
                         when 1 then  ' 5000 <= salary <= 10000'
                         else 'salary >= 10000'
  end;
  dbms_output.put_line(v_sal || ',' || v_temp);
end;
```

循环结构(三种)
方式1:loop ... exit when ... end loop;

```
declare
  v_i number(5) := 1;
begin
  loop
    -- 循环体
    dbms_output.put_line(v_i);
    -- 循环条件
  exit when v_i >= 100;
       --迭代条件
       v_i := v_i+1;
  end loop;
end
```
方式2 :while ... loop ... end loop;
```
declare
  v_i number(5) := 1;
begin
  while v_i <= 100 loop
    dbms_output.put_line(v_i);
    v_i := v_i+1;
  end loop;
end;
```

方式3:for i in ... loop ... end loop;
```
begin
  for c in 1..100 loop
    dbms_output.put_line(c);
  end loop;
end;

//或者反着来

begin
  for c in  reverse 1..100 loop
    dbms_output.put_line(c);
  end loop;
end;
```
如下程序输出1到100以内的质数
```
declare
  v_i number(3) := 2;
  v_j number(3) := 2;
  v_flag number(1) := 1;
begin
  while v_i <= 100 loop
    while v_j <= sqrt(v_i) loop
        if mod(v_i,v_j) = 0 then v_flag := 0;
        end if;
        v_j := v_j + 1;
      end loop;
      
      if v_flag = 1 then dbms_output.put_line(v_i);
      end if;
      
      v_j := 2;
      v_i := v_i+1;
      v_flag := 1;
  end loop;
end;

//或者使用for完成

declare
  v_flag number(1) := 1;
begin
  for v_i in 2..100 loop
    for v_j in 2..sqrt(v_i) loop
      if mod(v_i,v_j) = 0 then v_flag := 0;
      end if;
    end loop;
    if v_flag = 1 then dbms_output.put_line(v_i);
    end if;
    v_flag := 1;
  end loop;
end;
```

goto语句相当于break,exit退出
```
在语句中添加label,然后就可以使用goto调到指定位置

declare
  v_flag number(1) := 1;
begin
  for v_i in 2..100 loop
    for v_j in 2..sqrt(v_i) loop
      if mod(v_i,v_j) = 0 then v_flag := 0;
      goto lable;
      end if;
    end loop;
    <<lable>>
    if v_flag = 1 then dbms_output.put_line(v_i);
    end if;
    v_flag := 1;
  end loop;
end;
```

```
declare
  v_flag number(1) := 1;
begin
  for i in 1..100 loop
    if i = 50 then dbms_output.put_line('打印结束');
    exit;
    end if;
    dbms_output.put_line(i);
    
  end loop;
end;
```
游标的使用（类似java中的Iterator）

在PL/SQL程序中，对于处理**多行记录**的事务经常使用游标来使用

显示游标处理，四个步骤

- 定义游标
- 打开游标
- 提取游标数据
- 对该记录进行处理
- 继续处理，知道活动集合中记录
- 关闭游标

```
declare
  v_sal employees.salary%type;
  v_empid employees.employee_id%type;
  --定义游标
  cursor emp_sal_cursor is select employee_id,salary from employees where department_id = 80;
begin
  --打开游标
  open emp_sal_cursor;
  
  --提取游标
  fetch emp_sal_cursor into v_sal,v_empid;
  
  while emp_sal_cursor%found loop
    dbms_output.put_line('emp_id'||v_empid||','||'salary' || v_sal);
    fetch emp_sal_cursor into v_sal,v_empid;
  end loop;
  
  --关闭游标
  close emp_sal_cursor;
end;

```

游标属性

- %found 布尔属性,当最近一次读记录时成功返回，则值为true
- %notfound 布尔属性，与%found相反
- %isopen 布尔属性，当游标已经打开时返回true
- %rowcount 数字属性，返回已从游标中读取的记录数 

可以直接对游标使用for语句 (类似于Java中的Iterator)
```
declare
  v_sal employees.salary%type;
  v_empid employees.employee_id%type;
  --定义游标
  cursor emp_sal_cursor is select employee_id,salary from employees where department_id = 80;
begin
 
  for c in emp_sal_cursor loop
    dbms_output.put_line('emp_id'||c.employee_id||','||'salary' || c.salary);
    end loop;
end;
```

```
/*
利用游标批量更新员工工资
*/
declare
  cursor emp_sal_cursor is select employee_id,salary from employees;
  v_temp number(4,2);
  v_empid employees.employee_id%type;
  v_sal employees.salary%type;
  
begin
  open emp_sal_cursor;
  fetch emp_sal_cursor into v_empid,v_sal;
  
  while emp_sal_cursor%found loop
    if v_sal < 5000 then  v_temp := 0.05;
    elsif v_sal < 10000 then v_temp:= 0.03;
    elsif v_sal < 15000 then v_temp := 0.02;
    else v_temp := 0.01;
    end if;
    
    dbms_output.put_line(v_sal);
    update  employees
    set salary = salary + (1+v_temp)
    where employee_id = v_empid;
    
    fetch emp_sal_cursor into v_empid,v_sal;
   end loop;
   close emp_sal_cursor;
end;

//或者使用for循环实现（此种方法简单一些）

declare
  cursor emp_sal_cursor is select employee_id,salary from employees;
  v_temp number(4,2);
begin
  for c in emp_sal_cursor loop
    if c.salary < 5000 then v_temp := 0.05;
    elsif c.salary <10000 then v_temp := 0.03;
    elsif c.salary <15000 v_temp := 0.02;
    else v_temp := 0.01;
    end if
    
    update employees
    set salary = salary * (1+v_temp)
    where employee_id = c.employee_id;
  end loop;
   
end;
```

带参数的游标
```

declare
  cursor emp_sal_cursor(dept_id number,sal number) is 
  select employee_id,salary 
  from employees
  where department_id = dept_id and salry > sal;
  v_temp number(4,2);
begin
  for c in emp_sal_cursor(sal => 4000,dept => 80) loop
    if c.salary < 5000 then v_temp := 0.05;
    elsif c.salary <10000 then v_temp := 0.03;
    elsif c.salary <15000 v_temp := 0.02;
    else v_temp := 0.01;
    end if
    
    update employees
    set salary = salary * (1+v_temp)
    where employee_id = c.employee_id;
  end loop;
   
end;
```

隐式游标
```
begin 
   update employees
   set salary = salary + 10
   where employee_id = 1001;
   
   if sql%notfound then dbms_output.put_line('查无此人');
   end if;
end;

```

异常处理(三种方法)

预定义异常
```
declare
  v_salary employees.salary%type;
begin
  select salary into v_salary 
  from employees
  where employee_id > 100;
  
  dbms_output.put_line(v_salary);
exception
  when too_many_rows then dbms_output.put_line('输出行数太多了');
  when others then dbms_output.put_line('出现其他类型的异常了'); 
end;
```

非预定义异常
```
declare
--定义异常
  e_deleteid_exception exception;
  --将已经定义好的异常类型与标准的Oracle错误联系起来
  pragma exception_init(e_deleteid_exception,-2292);
begin
  delete from employees where employee_id = 100;
exception
  when e_deleteid_exception then dbms_output.put_line('违反完整性约束条件，故不可删除此此用户');
end;
```

用户自定义异常(相当于Java中的throw异常，只不过这里使用的是raise)
```
declare
  e_too_high_sal exception;
  v_sal employees.salary%type;
  
begin
  select salary into v_sal from employees where employee_id = 100;
  
  if v_sal > 10000 then 
    raise e_too_high_sal;
    end if;
    
exception
  when e_too_high_sal then dbms_output.put_line('工资太高了');
end;

```

前面知识都使与为存储函数(有返回值)和存储过程有(无返回值）

触发器的概念
