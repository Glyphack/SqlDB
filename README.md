# gosql

An example PostgreSQL implementation in Go.
Reference: https://notes.eatonphil.com/database-basics.html

## Example

```bash
$ git clone git@github.com:eatonphil/gosql
$ cd gosql
$ go run cmd/main.go
Welcome to gosql.
# CREATE TABLE users (id INT PRIMARY KEY, name TEXT, age INT);
ok
# \d users
Table "users"
  Column |  Type   | Nullable
---------+---------+-----------
  id     | integer | not null
  name   | text    |
  age    | integer |
Indexes:
        "users_pkey" PRIMARY KEY, rbtree ("id")

# INSERT INTO users VALUES (1, 'Corey', 34);
ok
# INSERT INTO users VALUES (1, 'Max', 29);
Error inserting values: Duplicate key value violates unique constraint
# INSERT INTO users VALUES (2, 'Max', 29);
ok
# SELECT * FROM users WHERE id = 2;
  id | name | age
-----+------+------
   2 | Max  |  29
(1 result)
ok
# SELECT id, name, age + 3 FROM users WHERE id = 2 OR id = 1;
  id | name  | ?column?
-----+-------+-----------
   1 | Corey |       37
   2 | Max   |       32
(2 results)
ok
```

## Architecture

* [cmd/main.go](./cmd/main.go)
  * Contains the REPL and high-level interface to the project
  * Dataflow is: user input -> lexer -> parser -> in-memory backend
* [lexer.go](./lexer.go)
  * Handles breaking user input into tokens for the parser
* [parser.go](./parser.go)
  * Matches a list of tokens into an AST or fails if the user input is not a valid program
* [memory.go](./memory.go)
  * An example, in-memory backend supporting the Backend interface (defined in backend.go)
