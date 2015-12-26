# Postgresql

将文件导入到数据库

```
\COPY table_name FROM 'filepath' WITH DELIMITER ' '
```

Alter修改列类型由character(255)为varchar(255)

```
ALTER TABLE mytable ALTER COLUMN hostname TYPE varchar(255);
```

Postgresql数据类型

|字段类型	|别名	|描述 |
|-- |-- |-- |
bigint	int8	有符号 8 字节整数
bit [ (n) ]		定长位串
bit varying [ (n) ]	varbit	变长位串
boolean	bool	逻辑布尔量（真/假）
bytea		二进制数据（"字节数组"）
character varying [ (n) ]	varchar(n)	变长字符串
character [ (n) ]	char(n)	定长字符串
text		变长字符串
inet		IPv4 或者 IPv6 网络地址
macaddr		MAC 地址
smallint	int,int2	有符号两字节整数
integer	int4	四字节长有符号整数
real	float4	单精度浮点数
double precision	float8	双精度浮点数字
date		日历日期（年，月，日）
timestamp [ (p) ] [ without time zone ]		日期和时间

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