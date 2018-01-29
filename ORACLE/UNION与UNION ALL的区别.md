### [SQL语句中：UNION与UNION ALL的区别](https://www.cnblogs.com/areyouready/p/7827779.html)

UNION用的比较多union all是直接连接，取到得是所有值，记录可能有重复   union 是取唯一值，记录没有重复  

1、UNION 的语法如下：

     [SQL 语句 1]
     
      UNION
      
     [SQL 语句 2]

2、UNION ALL 的语法如下：

     [SQL 语句 1]
     
      UNION ALL
      
     [SQL 语句 2]

效率：

UNION和UNION ALL关键字都是将两个结果集合并为一个，但这两者从使用和效率上来说都有所不同。

1、对重复结果的处理：UNION在进行表链接后会筛选掉重复的记录，Union All不会去除重复记录。

2、对排序的处理：Union将会按照字段的顺序进行排序；UNION ALL只是简单的将两个结果合并后就返回。

从效率上说，UNION ALL 要比UNION快很多，所以，如果可以确认合并的两个结果集中不包含重复数据且不需要排序时的话，那么就使用UNION ALL。


简要回答：

UNION去重且排序,　请注意，UNION内部的SELECT语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每条SELECT语句中的列的顺序必须相同。

UNION ALL不去重不排序



--------------


