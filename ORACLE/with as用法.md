[with as用法转载](https://blog.csdn.net/q649381130/article/details/77746271?locationNum=7&fps=1)
---------
工作中使用到，了解下使用

WITH AS短语，也叫做子查询部分，定义一个SQL片断后，该SQL片断可以被整个SQL语句所用到。有的时候，with as是为了提高SQL语句的可读性，减少嵌套冗余。

```sql
with A as (
  select  * 
  from user
) 
select * 
from A, customer 
where 
  customer.userid = user.id**
```

这个sql语句的意思是：先执行`select * from user`把结果放到一个临时表`A`中，作为全局使用。

通俗点讲，将需要频繁执行的slq片段加个别名放到全局中，后面直接调用就可以，这样减少调用次数，优化执行效率。

#### 使用with as有如下好处

1、可以轻松构建一个临时表，通过对这个表数据进行再处理。但是他比临时表更强大，临时表在会话结束才会自动被P清除，但with as临时表查询完成后就被清除了

2、复杂的查询会产生很大的sql，with as语法可以把一些公共查询提出来，也可以作为一个中间结果，可以使整个sql语句显得有条理些，提高可读性
 
