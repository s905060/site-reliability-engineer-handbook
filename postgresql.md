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

查看数据库中各个表的大小

`\d+`

启动postgresql

`bin/pg_ctl start -D pg_data`

停止postgresql

`/bin/pg_ctl stop -D pg_data`

单表备份

`pg_dump -t 表名 > backup.sql`

单表还原

`psql -f backup.sql`

清空表的所有内容，保留表结构

`truncate tablename`

创建用户&授权

#进入psql命令行
```
CREATE USER username WITH PASSWORD 'password';
GRANT SELECT ON TABLE  tablname TO username ;
```

查看用户列表

```
\du
#或者
select * from pg_shadow ;
```

查看指定用户的权限
```
#进入psql 交互命令行
select * from INFORMATION_SCHEMA.role_table_grants  where grantee='username';
```