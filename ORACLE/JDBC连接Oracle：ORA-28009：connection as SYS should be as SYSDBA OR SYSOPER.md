### JDBC连接Oracle

使用JDBC连接Oracle数据库时，由于sys是数据库内设的超用户，用此用户名 username=` "sys" `直接连接时会出现如下错误：

`java.sql.SQLException: ORA-28009: connection as SYS should be as SYSDBA or SYSOPER`

解决方法：

用户名设置为username =` "sys as sysdba" `，其他不变，比如：



```
package testoracle01;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

import org.junit.Test;

public class LinkOracle {
     @Test
     public void testJdbc() throws SQLException {
    	 String driver="oracle.jdbc.OracleDriver";
```
-------------
* 注意连接oracle是+ ` thin `
-------------

``` 
    	 String url="jdbc:oracle:thin:@192.168.201.129:1521:orcl";
```       
------------------------------------      
      String username= " sys as sysdba ";  
------------------------------------     
       
```       
    	 String password="root111";
    	 try {
			Class.forName(driver);
			Connection connection = DriverManager.getConnection(url, username, password);
			Statement statement = connection.createStatement();
			ResultSet rr = statement.executeQuery("select * from scott.emp");
			while(rr.next()) {
				System.err.println(rr.getObject(1)+" "+rr.getObject(2));
				
			}
		} catch (ClassNotFoundException e) {
			e.printStackTrace();
		}
     }
	
	
}


```
