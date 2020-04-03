# MySql

MySql.Data

```
//sql语句末尾需要带;
public delegate bool MySqlOpt_Query_PerRow(MySqlDataReader reader);
public delegate void MySqlOpt_Query_OneRow(object result);
class MySqlOpt
{
    MySqlCommand m_cmd = new MySqlCommand();
    public string Connect(string username, string password, string addr = "localhost", string dbname = null)
    {
        if (null != m_cmd.Connection)
        {
            if (m_cmd.Connection.State == System.Data.ConnectionState.Open)
                m_cmd.Connection.Close();
        }
        m_cmd.Connection = null;
        string connstr = "data source=" + addr;
        connstr = connstr + ";user id=" + username;
        connstr = connstr + ";password=" + password;
        if(null != dbname) connstr = connstr + ";database=" + dbname;
        connstr = connstr + ";pooling=false;charset=utf8"; 
        try
        {
            m_cmd.Connection = new MySqlConnection(connstr);
            m_cmd.Connection.Open();
        }catch(Exception e)
        {
            m_cmd.Connection = null;
            return e.ToString();
        }
        return null;
    }
    public string Exec(string sql)
    {
        if (null == m_cmd.Connection) return "未建立数据库连接！";
        try
        {
            m_cmd.CommandText = sql;
            int row = m_cmd.ExecuteNonQuery();
            return string.Format("{0} rows affected", row);
        }catch(Exception e)
        {
            return e.ToString();
        }
    }
    public string Query(string sql, MySqlOpt_Query_PerRow fun)
    {
        if (null == m_cmd.Connection) return "未建立数据库连接！";
        try
        {
            m_cmd.CommandText = sql;
            MySqlDataReader reader = m_cmd.ExecuteReader();
            while (reader.Read()) if (!fun(reader)) break;
            //遍历获取表头名
            //for (int col = 0; col < reader.FieldCount; col++)
            //{
            //    string data = reader.GetName(col);
            //}

            //获取某列数据
            //int id = reader.GetInt32("id");//参数为字段名，或者所在列数(列数从0开始)
            //string name = reader.GetString("name");
            //遍历数据
            //for (int col = 0; col < reader.FieldCount; col++)
            //{
            //    object data = reader[col];
            //}
            return null;
        }catch(Exception e)
        {
            return e.ToString();
        }
    }
    public string QueryOne(string sql, MySqlOpt_Query_OneRow fun)
    {
        if (null == m_cmd.Connection) return "未建立数据库连接！";
        try
        {
            m_cmd.CommandText = sql;
            object result = m_cmd.ExecuteScalar();
            fun(result);
            return null;
        }
        catch (Exception e)
        {
            return e.ToString();
        }
    }
}
```
