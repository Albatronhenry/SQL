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

