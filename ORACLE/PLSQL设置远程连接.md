### SQL设置远程连接

* 首先打开电脑，到pl/sql安装的指定目录【C:\oracle\product\10.2.0\db_1\NETWORK\ADMIN】找到【tnsnames.ora】


![oraclelj](https://github.com/Albatronhenry/UploadFile/blob/master/pic/oraclelj.png)


* 打开【tnsnames.ora】文件，增加需要远程连接的字符串。

------


ORCL =

  (DESCRIPTION =
  
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.201.129)(PORT = 1521))
    
    (CONNECT_DATA =
    
      (SERVER = DEDICATED)
      
      (SERVICE_NAME = orcl)
      
    )
    
  )

* 下面是添加的远程调用的内容：

```sql

DF_PRO_orcl =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 10.15.0.30)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = orcl)
    )
  )

```

![oraclelj](https://github.com/Albatronhenry/UploadFile/blob/master/pic/orclora.png)

--------

* 然后保存好，重新启动pl/sql，登陆页面即可显示：

![orapllogin](https://github.com/Albatronhenry/UploadFile/blob/master/pic/orapllogin.png)

* 然后输入账号密码登录即可进入

