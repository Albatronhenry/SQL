因项目实现需要，需要相同业务数据进行汇总小计
```sql
select decode(grouping(to_char(t.bill_type)),
              1,
              '小计',
              to_char(t.bill_type)) bill_type,
       sum(t.money) money,
       t.en_code,
       t.en_name
  from (常规业务查询语句体) t
 group by rollup((to_char(t.bill_type), t.money, t.en_code, t.en_name))

查询结果---------------------------------------
1	计划	373000	101001	***委办公厅本级
2	小计	373000		
3	支付	96600	101001	***委办公厅本级
4	支付	43973	101001	***委办公厅本级
5	小计	140573		
-----------------------------------------------
```

[因项目实现逻业务辑需要多次联合查询，导致卡慢，故使用with as 优化](https://www.cnblogs.com/mingforyou/p/8295239.html)
```sql
1-------------------------
--相当于建了个t临时表
--with as 相当于虚拟视图
----“一次分析，多次使用”
with t as (常规查询语句体) 
select * from t


2-------------------------
对于union all比较有用
因为union all的每个部分可能相同，但是如果每个部分都去执行一遍的话，则成本太高，所以可以使用with as短语，则只要执行一遍即可
如果with as短语所定义的表名被调用两次以上，则优化器会自动将with as短语所获取的数据放入一个temp表里，如果只是被调用一次，则不会

with
    sql1 as (select to_char(a) s_name from test_tempa),
    sql2 as (select to_char(b) s_name from test_tempb where not exists (select s_name from sql1 where rownum=1))
select * from sql1
union all
select * from sql2
union all
select 'no records' from dual
       where not exists (select s_name from sql1 where rownum=1)
       and not exists (select s_name from sql2 where rownum=1);

```
