# migrate

A migration helper written in Go. Use it in your existing Golang code
or run commands via the CLI.

This is lite version of mattes/migrate library for PostgreSQL only. Therefore, the following dependencies are removed:

* driver for Cassandra
* driver for MySQL
* driver for SQLite

```
GoCode   import github.com/newcloudtechnologies/migrate/migrate
```

__Features__

* Super easy to implement [Driver interface](http://godoc.org/github.com/newcloudtechnologies/migrate/driver#Driver).
* Gracefully quit running migrations on ``^C``.
* No magic search paths routines, no hard-coded config files.


## Available Drivers

 * [PostgreSQL](https://github.com/newcloudtechnologies/migrate/tree/master/driver/postgres)
 * Bash (planned)

Need another driver? Just implement the [Driver interface](http://godoc.org/github.com/newcloudtechnologies/migrate/driver#Driver) and open a PR.

## Usage in Go

See GoDoc here: http://godoc.org/github.com/newcloudtechnologies/migrate/migrate

```go
import "github.com/newcloudtechnologies/migrate/migrate"

// Import any required drivers so that they are registered and available
import _ "github.com/newcloudtechnologies/migrate/driver/postgres"

// use synchronous versions of migration functions ...
allErrors, ok := migrate.UpSync("driver://url", "./path")
if !ok {
  fmt.Println("Oh no ...")
  // do sth with allErrors slice
}

// use the asynchronous version of migration functions ...
pipe := migrate.NewPipe()
go migrate.Up(pipe, "driver://url", "./path")
// pipe is basically just a channel
// write your own channel listener. see writePipe() in main.go as an example.
```

## Migration files

The format of migration files looks like this:

```
001_initial_plan_to_do_sth.up.sql     # up migration instructions
001_initial_plan_to_do_sth.down.sql   # down migration instructions
002_xxx.up.sql
002_xxx.down.sql
...
```

Why two files? This way you could still do sth like
``psql -f ./db/migrations/001_initial_plan_to_do_sth.up.sql`` and there is no
need for any custom markup language to divide up and down migrations. Please note
that the filename extension depends on the driver.


## Alternatives

 * https://bitbucket.org/liamstask/goose
 * https://github.com/tanel/dbmigrate
 * https://github.com/BurntSushi/migration
 * https://github.com/DavidHuie/gomigrate
 * https://github.com/rubenv/sql-migrate


