# mysql

go get github.com/go-sql-driver/mysql

## 数据库连接

```
import (
	"database/sql"
	"fmt"

	_ "github.com/go-sql-driver/mysql"
)

func main() {
	//dbs := "root:root@tcp(123.123.123.123:3306)/testdb"
	dbs := "root:root@/testdb"
	db, err := sql.Open("mysql", dbs)
	defer db.Close()

	if err != nil {
		fmt.Println(err.Error())
        return
	}

	err = db.Ping()
	if err != nil {
		fmt.Println(err.Error())
        return
	}
}
```

## 查询

```
type User struct {
	Id   int    `db:"id"`
	Name string `db:"name"`
}


func Query(db *sql.DB) {
	rows, err := db.Query("select id, name from user")
	if err != nil {
		fmt.Println(err)
        return
	}
	if err == sql.ErrNoRows {
		fmt.Println("not found data")
		return
	}
	defer rows.Close()
	for rows.Next() {
		var user User
		err := rows.Scan(&user.Id, &user.Name)
		if err != nil {
			fmt.Println(err.Error())
		}
		fmt.Printf("user: %#v\n", user)
	}
}

func QueryOne(db *sql.DB) {
	row := db.QueryRow("select * from user where id=?", 1)
	var user User
	err := row.Scan(&user.Id, &user.Name)
	if err == sql.ErrNoRows {
		fmt.Println("not found data")
        return
	}
	if err != nil {
		fmt.Println(err.Error())
        return
	}
	fmt.Printf("QueryOne: %#v\n", user)
}

```

## 执行sql

```
func Exec(db *sql.DB, sql string) {

	result, err := db.Exec(sql)
	if err != nil {
		fmt.Println(err.Error())
        return
	}

	affected, err := result.RowsAffected()
	if err != nil {
		fmt.Println(err)
        return
	}
	fmt.Printf("affect rows:%d\n", affected)
}
```

## 预备语句

```
stmtIns, err := db.Prepare("INSERT INTO user VALUES( ?, ? )")
if err != nil {
	fmt.Println(err.Error())
}
defer stmtIns.Close()
_, err = stmtIns.Exec(10, "xx")
if err != nil {
	fmt.Println(err.Error())
}
```

```
stmtOut, err := db.Prepare("SELECT name FROM user WHERE id = ?")
if err != nil {
	fmt.Println(err.Error())
}
defer stmtOut.Close()
var name string
id := 3
err = stmtOut.QueryRow(id).Scan(&name)
if err != nil {
	fmt.Println(err.Error())
}
fmt.Println(name)
```