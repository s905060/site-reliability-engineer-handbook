# Postgresql

Import the file into the database

```
\COPY table_name FROM 'filepath' WITH DELIMITER ' '
```

Alter column types from the character (255) as varchar (255)

```
ALTER TABLE mytable ALTER COLUMN hostname TYPE varchar(255);
```

Postgresql Data Types

|Field Type	|Aliases	| Description |
|-- |-- |-- |
|bigint	|int8	|Signed 8-byte integer
|bit |[(n)]		|Fixed-length bit string
|bit varying [(n)]	|varbit	|Variable-length bit string
|boolean	|bool	|Boolean logic (true / false)
|bytea	|	|Binary data ("byte array")
|character varying [(n)]|	varchar (n)	|Variable-length strings
|character [(n)]	|char (n)	|Fixed-length string
|text		||Variable-length strings
|inet		||IPv4 or IPv6 network address
|macaddr	||	MAC address
|smallint	|int, int2	|Signed two-byte integer
|integer	|int4	|Signed four-byte integer
|real	|float4	|Single-precision floating-point number
|double precision	|float8	|Double-precision floating-point number
|date		||Calendar date (year, month, day)
|timestamp [(p)] [without time zone]	||	Date and Time

Check the size of each table in the database

`\d+`

Start postgresql

`bin/pg_ctl start -D pg_data`

Stop postgresql

`/bin/pg_ctl stop -D pg_data`

Single table backup

`pg_dump -t 表名 > backup.sql`

Restore a single table

`psql -f backup.sql`

Empty all the contents of the table, reserved table structure

`truncate tablename`

Create User & Authorization

```
# Enter the psql command line
CREATE USER username WITH PASSWORD 'password';
GRANT SELECT ON TABLE  tablname TO username ;
```

See the list of users

```
\du
# or
select * from pg_shadow ;
```

Specifies the user's permission

```
# Psql interactive command line enter
select * from INFORMATION_SCHEMA.role_table_grants  where grantee='username';
```