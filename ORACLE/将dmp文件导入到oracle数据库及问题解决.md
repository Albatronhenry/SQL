#### 流程
* 1、首先，我们可以先建立自己的一个用户表空间，创建表空间的格式如下：

```sql

create tablespace test(表空间的名字) 
datafile 'D:\oracle\product\10.2.0\userdata\test.dbf'  (这边可以写成oracle的某个路径下)
size 50m  （初始大小）
autoextend on;（自动扩展）

```

* 2、接下来，我们可以创建一个自己的用户，创建格式如下：

```sql

CREATE USER utest （用户名） 
IDENTIFIED BY upassword（密码）
DEFAULT TABLESPACE test(上面创建的表空间) 
TEMPORARY TABLESPACE temp;（临时表空间就写temp即可）

```


* 3、然后，我们需要给自己的用户赋予权限来管理自己的表空间

```sql

GRANT CONNECT TO utest;  
GRANT RESOURCE TO utest;  
GRANT dba TO utest;--dba为最高级权限，可以创建数据库，表等。

```

以上三条语句的执行环境都需要进入oralce之后

cmd下进入oracle的方式

sqlplus system/密码      回车即可



* 4、接下来我们就可以将我们的dmp文件导入到我们自己的表空间中了，导入方式

imp utest/upassword@SID full=y  file= d:\data\xxxx.dmp ignore=y



* 5 导入实例

imp utest/upassword  file=D:\20140227.dmp full=y ignore=y (将文件导入到我们自己新建的用户的表空间中)  

注意：这条语句的执行环境是刚进命令台时的环境

----------------

#### oracle导入提示“IMP-00010：不是有效的导出文件，头部验证失败”的解决方案

---------------

这是由于导出的dmp文件与导入的数据库的版本不同造成的
用Notepad++查看了dmp文件，在头部具修改成你将导入目标数据库的版本号
以下对应的版本号：
　　11g R2：V11.02.00
　　11g R1：V11.01.00

　　10g：V10.02.01


解决步骤：

* 1、查看dmp文件的版本号

* 2、查询导入oracle数据库的版本号

     通过 ` select * from v$version ` 查看版本号
     
* 3、修改dmp文件的版本号

* 4、重新执行导入sql即可完成导入工作。
