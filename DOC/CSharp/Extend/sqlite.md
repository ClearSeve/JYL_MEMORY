# SQLite

System.Data.SQLite

```
public delegate bool SqlLite_Query_PerRow(SQLiteDataReader reader);
public delegate void SqlLite_Query_OneRow(object result);
class SqlLiteOpt
{
    SQLiteCommand m_cmd = new SQLiteCommand();
    public string Open(string filePathName)
    {
        Close();
        try
        {
            m_cmd.Connection = new SQLiteConnection("data source=" + filePathName + ";version=3;");
            m_cmd.Connection.Open();
        }
        catch (Exception e)
        {
            m_cmd.Connection = null;
            return e.ToString();
        }
        return null;
    }
    public void Close()
    {
        if (null == m_cmd.Connection) return;
        if (m_cmd.Connection.State != System.Data.ConnectionState.Open) return;
        m_cmd.Connection.Close();
        m_cmd.Connection = null;
    }
    public string Exec(string sql)
    {
        if (null == m_cmd.Connection) return "未建立数据库连接！";
        try
        {
            m_cmd.CommandText = sql;
            int row = m_cmd.ExecuteNonQuery();
            return string.Format("{0} rows affected", row);
        }
        catch (Exception e)
        {
            return e.ToString();
        }
    }
    public string Query(string sql, SqlLite_Query_PerRow fun)
    {
        if (null == m_cmd.Connection) return "未建立数据库连接！";
        try
        {
            m_cmd.CommandText = sql;
            SQLiteDataReader reader = m_cmd.ExecuteReader();
            while (reader.Read()) if (!fun(reader)) break;
            reader.Close();
        }catch(Exception e)
        {
            return e.ToString();
        }
        return null;
    }
    public string QueryOne(string sql, SqlLite_Query_OneRow fun)
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
