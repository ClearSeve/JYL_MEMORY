# sqllite

String[] columnArray = new String[]{"id","name"};

## 创建基础类id

```
 class DatabaseHelper extends SQLiteOpenHelper {
    public DatabaseHelper(Context context, String pathname, SQLiteDatabase.CursorFactory factory, int version) {
        super(context, pathname, factory, version);
    }
    @Override
    public void onCreate(SQLiteDatabase db) {
        String sql = "CREATE TABLE \"dbname\" (" +
                "  \"id\" text NOT NULL," +
                "  \"name\" text NOT NULL")";
        db.execSQL(sql);
    }
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {}
}
```

## 查询

```
DatabaseHelper dbHelper = new DatabaseHelper(context, pathname,null,1);
SQLiteDatabase db = dbHelper.getWritableDatabase();
Cursor cursor = db.query(tableName, columnArray, null, null, null, null, null);
while(cursor.moveToNext()){
    String id = cursor.getString(cursor.getColumnIndex(columnArray[0])));
     String name = cursor.getString(cursor.getColumnIndex(columnArray[1])));
}
cursor.close();
db.close();
```

## 增加

```
SQLiteDatabase db = GetDB(context);
ContentValues values = new ContentValues();
values.put("id","0");
values.put("name","aa");
db.insert(tableName, null, values);
```

## 删除

```
DatabaseHelper dbHelper = new DatabaseHelper(context, pathname,null,1);
SQLiteDatabase db = dbHelper.getWritableDatabase();
db.delete(tableName, "id = ?", new String[]{data});
db.close();
```

## 修改

```
DatabaseHelper dbHelper = new DatabaseHelper(context, pathname,null,1);
SQLiteDatabase db = dbHelper.getWritableDatabase();
ContentValues values = combineContentValues(data);
db.update(tableName, values, "id = ?", new String[]{data});
db.close();
```
