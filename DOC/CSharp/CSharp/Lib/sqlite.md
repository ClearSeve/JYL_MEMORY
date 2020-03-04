# sqlite

System.Data.SQLit

## 操作定义

```
delegate void ProcPerRow(SQLiteDataReader reader);
class SqlLiteOpt
{
    string m_filePathName;
    SQLiteConnection m_cn = null;
    SQLiteCommand m_cmd = new SQLiteCommand();
    public bool Open(string filePathName)
    {
        Close();
        m_filePathName = filePathName;
        try
        {
            m_cn = new SQLiteConnection("data source=" + m_filePathName + ";version=3;");
            m_cn.Open();
            m_cmd.Connection = m_cn;
        }
        catch(Exception e)
        {
            return false;
        }
        return true;
    }
    public void Close()
    {
        if (null == m_cn) return;
        if (m_cn.State != System.Data.ConnectionState.Open) return;
        m_cn.Close();
    }
    public int Exec(string sql)
    {
        m_cmd.CommandText = sql;
        return m_cmd.ExecuteNonQuery();
    }
    public void Query(string sql, ProcPerRow proc)
    {
        m_cmd.CommandText = sql;
        SQLiteDataReader reader = m_cmd.ExecuteReader();
        while (reader.Read())
        {
            proc(reader);
            //for (int col = 0; col < reader.FieldCount; col++)
            //{
            //    object data = reader[col];
            //}
        }
        reader.Close();
    }
}
```

## 使用

```
void Write()
{
    SqlLiteOpt m_opt = new SqlLiteOpt();
    if (!m_opt.Open("D:\\test.db")) return;
    m_opt.Exec("CREATE TABLE IF NOT EXISTS test(id int,name varchar(4))");
    m_opt.Exec("INSERT INTO test VALUES(0,'aa')");
    m_opt.Exec("INSERT INTO test VALUES(1,'bb')");
    m_opt.Close();
    MessageBox.Show("写入完成");
}
void Read()
{
    SqlLiteOpt m_opt = new SqlLiteOpt();
    if (!m_opt.Open("D:\\test.db")) return;
    m_opt.Query("SELECT * FROM test", (SQLiteDataReader reader)=> {
        for (int col = 0; col < reader.FieldCount; col++)
        {
            object data = reader[col];
        }
    });
    m_opt.Close();
}
```
