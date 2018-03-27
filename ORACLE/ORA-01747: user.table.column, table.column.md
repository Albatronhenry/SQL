### [java.sql.SQLSyntaxErrorException: ORA-01747: user.table.column, table.column](https://blog.csdn.net/panjie510/article/details/7792010)

错误提示如标题：

* 1.可能是在oracle或其他数据库中使用了保留的关键字，可以通过
select *

from v$reserved_words
where keyword

in(

select COLUMN_NAME

from all_tab_columns

where table_name = 'USERINFO' and owner='PANJIE');查询，userinfo为你的表名，设置大写，PANJIE为你的用户名，设置大写

* 2.你的sql语句可能多了个逗号，或者其中插入的有null值(本次错误是因为多了逗号)
