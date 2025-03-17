
We've added two functions at the root level.


initRedis to initialise redis : 

```
func initRedis() *redis.Pool {
	redisPool := &redis.Pool{
		MaxIdle: 10,
		Dial: func() (redis.Conn, error) {
			return redis.Dial("tcp", os.Getenv("REDIS"))
		},
	}
	return redisPool
}
```

So, this is the initialising of redisPool, at a point of time, we have a pool of connections to our redis server.

```
func initSession() *scs.SessionManager {
	//setup session
	session := scs.New()
	session.Store = redisstore.New(initRedis())
	session.Lifetime = 24 * time.Hour
	session.Cookie.Persist = true
	session.Cookie.SameSite = http.SameSiteLaxMode
	session.Cookie.Secure = true

	return session
}
```

So, in this setup we're using the scs.New() package for session.

Store for session := redisstore.New(initRedis())
Lifetime for session = 24 Hrs
Cookie persisting = true, we want the cookie to be in b/w 


Now, 

Cookie Persistence Has Implications : 
Both on frontend and backend
When session.Cookie.Persist = true:

- **When `session.Cookie.Persist = true`**:
    
    - User logs in and receives a session cookie with an expiration date set by the server.
    - User closes the browser.
    - User reopens the browser and navigates to the application.
    - The browser sends the persisted session cookie with the request.
    - The server recognizes the session cookie and the user remains logged in.
- **When `session.Cookie.Persist = false`**:
    
    - User logs in and receives a session cookie without an expiration date (session cookie).
    - User closes the browser.
    - The session cookie is deleted from the browser's memory.
    - User reopens the browser and navigates to the application.
    - The browser does not send the session cookie (since it was deleted).
    - The server does not recognize the user, and the user needs to log in again.


Same-SiteLAX : 

### Example Scenario with `SameSite=Lax`:

- **Scenario**: A user is logged in to your web application.
- **User Action**: The user clicks a link to your web application from another site (top-level navigation).
- **Result**: The browser sends the session cookie with the request, allowing the user to remain logged in.

However, if your web application’s content (e.g., images, iframes) is embedded on another site:

- **Scenario**: Another site embeds an image or iframe from your web application.
- **User Action**: The browser attempts to load the embedded content.
- **Result**: The browser does not send the session cookie with these subrequests, helping to prevent CSRF attacks.


Adding appConfigd : :


```
package main

import (
	"database/sql"
	"log"
	"sync"

	"github.com/alexedwards/scs/v2"
)

type Config struct {
	Session  *scs.SessionManager
	DB       *sql.DB
	InfoLog  *log.Logger
	ErrorLog *log.Logger
	Wait     *sync.WaitGroup
}

```

This is how main file looks : 

```
func main() {

	//connect to the database
	db := initDB()

	//Since people are going to be logging in, create sessions
	session := initSession()

	//Create loggers
	infoLog := log.New(os.Stdout, "INFO\t", log.Ldate|log.Ltime)
	errorLog := log.New(os.Stderr, "ERROR\t", log.Ldate|log.Ltime|log.Lshortfile)

	//create some channels

	//Create atleast one waitgroup.
	wg := sync.WaitGroup{}

	//Setup the application config
	app := Config{
		Session:  session,
		DB:       db,
		InfoLog:  infoLog,
		ErrorLog: errorLog,
		Wait:     &wg,
	}

	//Setup mail

	//Listen for webconnections
}
```

