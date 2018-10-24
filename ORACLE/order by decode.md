[order by decode函数确保想要的数据在第*行](https://blog.csdn.net/cainiaowys/article/details/6614506)

未使用该函数之前
```sql
select * from TAB_MON_MIDIFY_2018 

64, '南昌', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                   
70, '南昌', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                   
37, '新建', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                       
37, '青山湖', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                   
24, '安义', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                         
22, 'JXCZ', 'JDBC Thin Client', 'uf1054201601183', '10.*.7.*     
```

使用后
```sql
select * from TAB_MON_MIDIFY_2018 t order by  decode(username,'合计','1','安义',2,username)

22, '合计', 'JDBC Thin Client', 'uf1054201601183', '10.*.7.*
24, '安义', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                              
22, 'JXCZ', 'JDBC Thin Client', 'uf1054201601183', '10.*.7.*                                                                                                     
22, 'JXCZ', 'JDBC Thin Client', 'uf1054201601183', '10.*.7.*                                                                                                                                                                                                                                                                                                                                                                                                                     
37, '青山湖', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                        
37, '新建', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1 
64, '南昌', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1  
70, '南昌', 'JDBC Thin Client', 'WIN-I5C5WDJEJFB', '127.0.0.1                                                                                                        
```

