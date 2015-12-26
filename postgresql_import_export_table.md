# Postgresql Import Export Table

###Export Table
```
pg_dump - h localhost - U postgres (username)  (when the same default user name) database name   - t table (table name)  >  dump . SQL
```

###Import Table
```
psql - f dump . SQL
```