[Oracle恢复某个表](https://blog.csdn.net/joyksk/article/details/78866018)

工作中由于数据问题删除了相关数据，后又需要恢复，采用表闪回

1、当想恢复某张表中的数据到某个时间时，可执行下面语句：
```sql
SQL>flashback table sysuser to timestamp to_date('2017-12-21 10:02:55','YYYY-MM-DD HH24:MI:SS');
```
2、若出现ORA-08189异常，则执行以下语句授权即可：
```sql
alter table sysuser enable row movement;
```
3、在执行1的flashback语句即可成功。
做个记录，以便后续之用。



                                                                                                                                                                                                                                                                                                                                                                                                   
