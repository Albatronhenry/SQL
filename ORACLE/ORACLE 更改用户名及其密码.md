[ORACLE 更改用户名及其密码](https://blog.csdn.net/zxr85/article/details/46646551)
-----------------

* 1、用sys角色账号进入，然后查询有哪些用户：

  * SELECT USERNAME,USER_ID FROM DBA_USERS    或者(SELECT * FROM user$)


* 2、找到需要修改的用户（user#字段是唯一标识）

  * SELECT USERNAME,USER_ID FROM DBA_USERS WHERE USE_ID=71   或者（SELECT * FROM user$ WHERE user#=71）



* 3、修改需要更改的用户名

  * UPDATE DBA_USERS SET USER_NAME=‘新的用户名’ WHERE USER_ID=71；   或者（UPDATE USER$ SET NAME=‘新的用户名’ WHERE user#=71;）
  * COMMIT;             一定要提交,不然更新不生效



* 4、强制刷新

  * ALTER SYSTEM CHECKPOINT;
  * ALTER SYSTEM FLUSH SHARED_POOL;

* 5、再将新的用户名对应的密码修改下（否则无法登录）

  * ALTER USER 新用户名 IDENTIFIED BY '密码';
