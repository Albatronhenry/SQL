# oracle
### 在Linux下解决方案
遇到使用sysdba登录出现ORA-01012
可以采用以下四种思路处理，治本的办法还是修改processes值，或者禁止一些异常访问

 1. 等访问数下降下来再登录
 2. 

 >  `sqlplus “/ as sysdba”`
> 	`shutdown abort		`

 3. 
> `ps -ef|grep ora_dbw0_$ORACLE_SID`
>    `kill -9 pid`

 

  4.kill掉一些不重要的session
 rac 环境节点三 出现如下 问题
那么重新启动实例 就好了。
kill oracle 进程 或者关掉oracle

> `ps -ef|grep ora_dbw0_$ORACLE_SID` 
> `kill  -9 pid`

### 检查Oracle DB监听器是否正常

重新启动oracle(`下面为关键代码`）
回到终端机模式，输入：

> ` lsnrctl status`

检查看看监听器是否有启动
如果没有启动，可以输入：

> ` lsnrctl start`

启动监听器
> `sqlplus sys as sysdba `  
> `startup;  `

[reference website1](http://www.cnblogs.com/mchina/archive/2012/11/27/2782993.html)

[reference website2](http://blog.csdn.net/u010098331/article/details/50767102)


