### nvl | to_char | to_date()

#### [oracle] to_date() 与 to_char() 日期和字符串转换
* to_date("要转换的字符串","转换的格式")   

    两个参数的格式必须匹配，否则会报错。即按照第二个参数的格式解释第一个参数。
    
```sql

--dual
select to_date('2018-01-22,11:22:30','yyyy-mm-dd,hh24:mi:ss ') from dual 

```

* to_char(日期,"转换格式" ) 即把给定的日期按照“转换格式”转换

```sql

select to_char(sysdate,'yy-mm-dd hh24:mi:ss') from dual

```


参数详解
---------

转换的格式：

表示year的：y  表示年的最后一位 yy 表示年的最后2位 yyy 表示年的最后3位 yyyy 用4位数表示年

表示month的：mm 用2位数字表示月；mon 用简写形式 比如11月或者nov ；month 用全称 比如11月或者november

表示day的：dd 表示当月第几天；ddd表示当年第几天；dy 当周第几天 简写  比如星期五或者fri；day当周第几天 全写

比如星期五或者friday。

表示hour的：` hh ` 2位数表示小时 12进制； ` hh24 ` 2位数表示小时 24小时

表示minute的：mi 2位数表示分钟

表示second的：ss 2位数表示秒 60进制

表示季度的：q 一位数 表示季度 （1-4）

另外还有ww 用来表示当年第几周 w用来表示当月第几周。

24小时制下的时间范围：00：00：00-23：59：59

12小时制下的时间范围：1：00：00-12：59：59

补充：

当前时间减去7分钟的时间
select sysdate,sysdate - interval '7' MINUTE from dual
当前时间减去7小时的时间
select sysdate - interval '7' hour from dual
当前时间减去7天的时间
select sysdate - interval ’7’ day from dual
当前时间减去7月的时间
select sysdate,sysdate - interval '7' month from dual
当前时间减去7年的时间
select sysdate,sysdate - interval '7' year from dual
时间间隔乘以一个数字
select sysdate,sysdate - 8*interval '7' hour from dual

Dual伪列

含义解释：

Dual 是 Oracle中的一个实际存在的表，任何用户均可读取，常用在没有目标表的select语句块中。

比如，我要获得系统时间，则用“select sysdate from dual” 则返回系统当前的时间：2008-11-07 9:32:49，不同系统可能返回日期的格式不一样。

"select user from dual"则返回当前连接的用户。如果是"select 1+2 from dual"，则返回结果：3

-------------


* oracle中nvl()函数

 * 1.NVL函数

NVL函数的格式如下：NVL(expr1,expr2)

含义是：如果oracle第一个参数为空那么显示第二个参数的值，如果第一个参数的值不为空，则显示第一个参数本来的值。

例如：

SQL> select ename,NVL(comm, -1) from emp;

 

ENAME NVL(COMM,-1)

------- ----

SMITH -1

ALLEN 300

WARD 500

JONES -1

MARTIN 1400

BLAKE -1

FORD -1

MILLER -1

其中显示-1的本来的值全部都是空值的

 

 * 2 NVL2函数

NVL2函数的格式如下：NVL2(expr1,expr2, expr3)

含义是：如果该函数的第一个参数为空那么显示第二个参数的值，如果第一个参数的值不为空，则显示第三个参数的值。SQL> select ename,NVL2(comm,-1,1) from emp;

 

ENAME NVL2(COMM,-1,1)

------- -----

SMITH 1

ALLEN -1

WARD -1

JONES 1

MARTIN -1

BLAKE 1

CLARK 1

SCOTT 1

上面的例子中。凡是结果是1的原来都不为空，而结果是-1的原来的值就是空。

 

 * 3. NULLIF函数

NULLIF(exp1,expr2)函数的作用是如果exp1和exp2相等则返回空(NULL)，否则返回第一个值。
下面是一个例子。使用的是oracle中HR schema，如果HR处于锁定，请启用

这里的作用是显示出那些换过工作的人员原工作，现工作。

SQL> SELECT e.last_name, e.job_id,j.job_id,NULLIF(e.job_id, j.job_id) “Old Job ID”

FROM employees e, job_history j

WHERE e.employee_id = j.employee_id

ORDER BY last_name;

 

LAST_NAME JOB_ID JOB_ID Old Job ID

----------------- ------- ------- -------

De Haan AD_VP IT_PROG AD_VP

Hartstein MK_MAN MK_REP MK_MAN

Kaufling ST_MAN ST_CLERK ST_MAN

Kochhar AD_VP AC_MGR AD_VP

Kochhar AD_VP AC_ACCOUNT AD_VP

Raphaely PU_MAN ST_CLERK PU_MAN

Taylor SA_REP SA_MAN SA_REP

Taylor SA_REP SA_REP

Whalen AD_ASST AC_ACCOUNT AD_ASST

Whalen AD_ASST AD_ASST

可以看到凡是employee。job_id和job_histroy.job_id相等的，都会在结果中输出NULL即为空，否则显示的是employee。job_id

 * 4.Coalesce函数

Coalese函数的作用是的NVL的函数有点相似，其优势是有更多的选项。

格式如下：

Coalesce(expr1, expr2, expr3….. exprn)

表示可以指定多个表达式的占位符。所有表达式必须是相同类型，或者可以隐性转换为相同的类型。
返回表达式中第一个非空表达式，如有以下语句： 　　SELECT COALESCE(NULL,NULL,3,4,5) FROM dual 　　其返回结果为：3
如果所有自变量均为 NULL，则 COALESCE 返回 NULL 值。 　　COALESCE(expression1,...n) 与此 CASE 函数等价：
这个函数实际上是NVL的循环使用，在此就不举例子了
