###  [Oracle参数processes等设置过小](http://www.linuxidc.com/Linux/2013-08/88503.htm)

需要调整它，processes当前的值为：

查看对应值:show parameter  ` XXX `

```sql

SQL>show parameter processes;

```
修改对应值: alter system set ` XXX `=值的大小 scope=作用域;

```sql

SQL>alter system set sort_area_size=1048576 scope=spfile;

```

--------

#### [scope作用域3种:](http://www.linuxidc.com/Linux/2013-08/88503.htm)

Scope（默认：both）：有如下3个取值：

-- memory 表示只在内存中修改，实例重启后失效；

-- spfile表示只在spfile中修改，只有当重启重新读取spfile之后才生效；

-- both表示同时在memory和spfile中修改（推荐）

Sid (默认：*）：该选项针对RAC，默认为*，表示RAC的所有实例同时修改，如果不想全部修改，用Sid指定Oracle实例即可。

------

#### [oracle 重启与关闭](http://blog.csdn.net/leetrong/article/details/5659134)

---------------

* 一.在命令行中启动/停止/查看监听程序

1.cmd

2.使用“Lsnrctl start” 启动监听程序

3.使用“Lsnrctl stop” 停止监听程序

4. 使用“Lsnrctl status” 查看监听程序

* 二.准备启动和关闭数据库

```sql

1.cmd

2.SQLPLUS /nolog

3. connect system/password AS SYSDBA

```

     启动数据库有3中模式

     Nomount模式：启动例程，但不装载数据库，即只完成启动步骤的第1步；

     Mount 模式：启动例程，并装载数据库，但不打开数据库，即只完成启动步骤的第1，2步；

    Open模式：启动例程，装载数据库，打开数据库

```sql

sql>startup Nomount

sql>alert database mount

sql>alert database open

```

    

* 三.数据库的启动（STARTUP）

启动一个数据库需要三个步骤：

1、 创建一个Oracle实例（非安装阶段）

2、 由实例安装数据库（安装阶段）

3、 打开数据库（打开阶段）

在Startup命令中，能够通过不同的选项来控制数据库的不同启动步骤。


1、STARTUP NOMOUNT

NOMOUNT选项仅仅创建一个Oracle实例。读取init.ora初始化参数文档、启动后台进程、初始化系统全局区（SGA）。Init.ora文档定义了实例的配置，包括内存结构的大小和启动后台进程的数量和类型等。实例名根据 Oracle_SID配置，不一定要和打开的数据库名称相同。当实例打开后，系统将显示一个SGA内存结构和大小的列表，如下所示：

```sql

SQL> startup nomount

```

ORACLE 例程已启动。

Total System Global Area 35431692 bytes

Fixed Size 70924 bytes

Variable Size 18505728 bytes

Database Buffers 16777216 bytes

Redo Buffers 77824 bytes

2、STARTUP MOUNT

该命令创建实例并且安装数据库，但没有打开数据库。Oracle系统读取控制文档中关于数据文档和重作日志文档的内容，但并不打开该文档。这种打开方式常在数据库维护操作中使用，如对数据文档的更名、改变重作日志连同打开归档方式等。在这种打开方式下，除了能够看到SGA系统列表以外，系统还会给出"数据库装载完毕"的提示。

3、STARTUP

该命令完成创建实例、安装实例和打开数据库的任何三个步骤。此时数据库使数据文档和重作日志文档在线，通常还会请求一个或是多个回滚段。这时系统除了能够看到前面Startup Mount方式下的任何提示外，还会给出一个"数据库已打开"的提示。此时，数据库系统处于正常工作状态，能够接受用户请求。

假如采用STARTUP NOMOUNT或是STARTUP MOUNT的数据库打开命令方式，必须采用ALTER DATABASE命令来执行打开数据库的操作。例如，假如您以STARTUP NOMOUNT方式打开数据库，也就是说实例已创建，但是数据库没有安装和打开。这是必须运行下面的两条命令，数据库才能正确启动。

```sql

ALTER DATABASE MOUNT;

ALTER DATABASE OPEN;

```

而假如以STARTUP MOUNT方式启动数据库，只需要运行下面一条命令即能够打开数据库：

```sql

ALTER DATABASE OPEN.

```

* 四.数据库的关闭（SHUTDOWN）

对于数据库的关闭，有四种不同的关闭选项，下面对其进行一一介绍。

1、SHUTDOWN NORMAL

这是数据库关闭SHUTDOWN命令的确省选项。也就是说假如您发出SHUTDOWN这样的命令，也即是SHUTDOWN NORNAL的意思。

发出该命令后，任何新的连接都将再不允许连接到数据库。在数据库关闭之前，Oracle将等待现在连接的任何用户都从数据库中退出后才开始关闭数据库。采用这种方式关闭数据库，在下一次启动时无需进行任何的实例恢复。但需要注意一点的是，采用这种方式，也许关闭一个数据库需要几天时间，也许更长。

2、SHUTDOWN IMMEDIATE

这是我们常用的一种关闭数据库的方式，想很快地关闭数据库，但又想让数据库干净的关闭，常采用这种方式。

当前正在被Oracle处理的SQL语句立即中断，系统中任何没有提交的事务全部回滚。假如系统中存在一个很长的未提交的事务，采用这种方式关闭数据库也需要一段时间（该事务回滚时间）。系统不等待连接到数据库的任何用户退出系统，强行回滚当前任何的活动事务，然后断开任何的连接用户。

3、SHUTDOWN TRANSACTIONAL

该选项仅在Oracle 8i后才能够使用。该命令常用来计划关闭数据库，他使当前连接到系统且正在活动的事务执行完毕，运行该命令后，任何新的连接和事务都是不允许的。在任何活动的事务完成后，数据库将和SHUTDOWN IMMEDIATE同样的方式关闭数据库。

4、SHUTDOWN ABORT

这是关闭数据库的最后一招，也是在没有任何办法关闭数据库的情况下才不得不采用的方式，一般不要采用。假如下列情况出现时能够考虑采用这种方式关闭数据库。

1、 数据库处于一种非正常工作状态，不能用shutdown normal或shutdown immediate这样的命令关闭数据库;

2、 需要立即关闭数据库；

3、 在启动数据库实例时碰到问题；

任何正在运行的SQL语句都将立即中止。任何未提交的事务将不回滚。Oracle也不等待现在连接到数据库的用户退出系统。下一次启动数据库时需要实例恢复，因此，下一次启动可能比平时需要更多的时间。




