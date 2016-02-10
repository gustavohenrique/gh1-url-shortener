## About

It's is my first personal web app written in Go. I have studied Go for 2 weeks and I learned so much about it.
In this first version I learned about:

* Database operations
* API using net/http and Gorilla Mux
* TDD and BDD in Go
* Godeps
* And much more

If you have tips or advices to improve my code please, share with me.

## Screenshots

I developed 2 distinct UI. The first uses AngularJS + Semantic-UI and the second React + Redux + Bootstrap/Material Design Theme.

### AngularJS + Semantic-UI

![AngularJS](screenshots/angularjs.png)

### React + Redux + Bootstrap with Material theme

![React](screenshots/react.png)

## Dev Environment

### 1. Setup Go environment

```sh
GOPATH=$PWD
PATH=$PATH:$GOPATH/bin
go get github.com/tools/godep
godep restore src/gh1
```

### 2. Run a PostgreSQL container

```
$ docker run --name some-postgres -e POSTGRES_PASSWORD=root -d postgres
```

### 3. Create database/table

```bash
$ docker run -it -P --link some-postgres:postgres --rm postgres sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres'
```

Inside the psql:

```sql
postgres=# create database gh1_test;
postgres=# \c gh1_test
gh1_test=# create table websites (
	id serial not null primary key,
	title varchar(250) not null,
	long_url varchar(1000) not null,
	code varchar(10) not null,
	created_at date default current_date,
	last_access date default current_date,
	hits integer default 0,
	is_visible boolean not null default true,
	tags varchar(100)[] default '{}',
	constraint long_url unique(long_url),
	constraint code unique(code)
);
```

## Running on Docker

```
docker run -d --name gh1 -e 'DATABASE_URL=user=gh1 dbname=gh1 password=root host=postgres.docker sslmode=disable' -e 'PORT=8002' -p 8002:8002 -v "$PWD":/usr/lib/go/src/gh1 gh1
```

## License

MIT