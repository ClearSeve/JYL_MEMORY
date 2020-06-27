# msql

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

public class dbOpt {
    Connection conn;
    Statement stmt;
    ResultSet result;	
    
    
	public dbOpt(String DBName, String DBUser, String DBPass){
        try {
        	Class.forName("com.mysql.cj.jdbc.Driver");//com.mysql.jdbc.Driver
        	String connMsg = "jdbc:mysql://localhost:3306/" + DBName + "?user=" + DBUser + "&password=" + DBPass;
        	conn = DriverManager.getConnection(connMsg);
        }catch(Exception e){System.out.println(e.toString());}
    }
	
	
    public ResultSet Query(String SQLQuery){
        try {
        	stmt = conn.createStatement();
            result = stmt.executeQuery( SQLQuery );
        }
        catch( Exception e ){System.out.println(e.toString());}
    	return result;
    }
    public void exec(String SQLQuery){
        try {
        	stmt = conn.createStatement();
            stmt.execute( SQLQuery );
        }
        catch( Exception e ){System.out.println(e.toString());}
    }
    
    
    public void close(){
    	try {
        	stmt.close();
            conn.close();
        }
        catch(Exception e){System.out.println(e.toString());}
    }
}

===============??
dbOpt  db = new dbOpt("testdb","root","root");
ResultSet rs =db.Query("select * from testTable"); 		
while(rs.next()){
	int id = rs.getInt("id");
	System.out.println(id);
}
    		
db.exec("insert into testTable(id) values(23)");
```
