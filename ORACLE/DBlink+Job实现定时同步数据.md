因业务需要，确保我方数据库数据稳定，针对不用实时查询的数据进行本地数据存储，故完成此需求；
[参考链接](https://blog.csdn.net/u013310119/article/details/76618977)

1.创建db_llink(可以通过pl/sql直接创建)
```sql
create database link ZHZJ_LINK
  connect to ZHZJ2019
  using '(DESCRIPTION = 
  (ADDRESS_LIST = (ADDRESS = (PROTOCOL = TCP)(HOST = 10.*.*.9 )(PORT = 1521)) )  
  (CONNECT_DATA = (SERVICE_NAME = ZHZJGL) 
  ) 
  )';
```

2.创建存储过程(注意存储过程的格式，不然就会报错)
```sql
--执行下面存储过程
create or replace procedure zfxt_from_zhzj2019 as
begin
  delete from acc_ele_account2019;
  insert into acc_ele_account2019 
  select *  from acc_ele_account@ZHZJ_LINK;
  commit;
exception
 when others
      then
     dbms_output.put_line('Exceptionhappened,执行插入发生异常，数据回滚');
     rollback;
end;
```

3.创建自动任务job(plsql--new--job)
```sql
begin
  sys.dbms_job.change(job => 86,
                      what => 'test_job_zhzj20190522;',
                      next_date => to_date('22-05-2019 14:23:02', 'dd-mm-yyyy hh24:mi:ss'),
                      interval => 'sysdate+2/24/60');
  commit;
end;
/
-------------------------
--创建job
What      填入 zfxt_from_zhzj2019;
Next date 填入 SYSDATE
Interval  填入 sysdate+2/24/60


ps:下面来讲讲定时任务的时间间隔怎么算的。
第一种调度任务需求的日期算法比较简单，即'SYSDATE+n',这里n是一个以天为单位的时间间隔。
描述 Interval参数值
每天运行一次    'SYSDATE + 1'
每小时运行一次    'SYSDATE + 1/24'
每10分钟运行一次    'SYSDATE+ 10/（60*24）'
每30秒运行一次    'SYSDATE+ 30/(60*24*60)'
每隔一星期运行一次    'SYSDATE + 7'
不再运行该任务并删除它 NULL
第二种调度任务需求相对于第一种就需要更复杂的时间间隔（interval）表达式
描述 INTERVAL参数值
每天午夜12点   'TRUNC(SYSDATE +1)'
每天早上8点30分   'TRUNC(SYSDATE + 1) +（8*60+30）/(24*60)'
每星期二中午12点     'NEXT_DAY(TRUNC(SYSDATE ), ''TUESDAY'' ) + 12/24'
每个月第一天的午夜12点   'TRUNC(LAST_DAY(SYSDATE ) + 1)'
每个季度最后一天的晚上11点   'TRUNC(ADD_MONTHS(SYSDATE + 2/24, 3 ), 'Q' ) -1/24'
每星期六和日早上6点10分    'TRUNC(LEAST(NEXT_DAY(SYSDATE, ''SATURDAY"),NEXT_DAY(SYSDATE, "SUNDAY"))) +（6×60+10）/（24×60）'
```
