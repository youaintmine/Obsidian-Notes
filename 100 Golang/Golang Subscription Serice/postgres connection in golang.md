
in the main func, 

func main() {

	//connect to the database
	initDB()

	//Since people are going to be logging in, create sessions

	//create some channels

	//Create atleast one waitgroup.

	//Setup the application config

	//Setup mail

	//Listen for webconnections
}

Ignore all the other comments.

Now,

```
func initDB() *sql.DB {
	conn := connectToDB

	if conn == nil {
		log.Panic("Can't connect to Database")
	}

}
```

And there will be a loop to connect to the DB.

Notice, that we're returning a different type of pointer. 

```
*sql.DB pointer.
```


MORE ABOUT sql

In Go, `*sql.DB` is a type from the `database/sql` package that represents a database handle. It provides methods to interact with a SQL database, such as opening connections, executing queries, and managing transactions.

Here's a brief overview of `*sql.DB`:

### Overview of `*sql.DB`

- **Connection Pooling**: `*sql.DB` manages a pool of connections to the database, which allows for efficient reuse of connections and handles multiple concurrent requests.
- **Opening a Database**: Use `sql.Open` to create a `*sql.DB`. This doesn't establish any connections to the database; it just initializes the object.
- **Querying the Database**: Methods like `Query`, `QueryRow`, `Exec`, and `Prepare` are used to interact with the database.
- **Connection Management**: You can control the connection pool with methods like `SetMaxOpenConns`, `SetMaxIdleConns`, and `SetConnMaxLifetime`.

### Example Usage

Here's a basic example of using `*sql.DB` in a Go program:

go

Copy code

`package main  import (     "database/sql"     "fmt"     _ "github.com/lib/pq" // Import the database driver for PostgreSQL )  func main() {     // Open a connection to the database     db, err := sql.Open("postgres", "user=postgres password=mysecretpassword dbname=mydb sslmode=disable")     if err != nil {         fmt.Println("Error opening database:", err)         return     }     defer db.Close()      // Execute a query     rows, err := db.Query("SELECT id, name FROM users")     if err != nil {         fmt.Println("Error executing query:", err)         return     }     defer rows.Close()      // Process the query results     for rows.Next() {         var id int         var name string         if err := rows.Scan(&id, &name); err != nil {             fmt.Println("Error scanning row:", err)             return         }         fmt.Println(id, name)     }     if err := rows.Err(); err != nil {         fmt.Println("Error with rows:", err)     } }`
