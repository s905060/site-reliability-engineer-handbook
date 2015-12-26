# Practical PostgreSQL

###Using Transaction Blocks

Transaction blocks are explicitly started with the BEGIN SQL command. This keyword may optionally be followed by either of the noise terms WORK or TRANSACTION, though they have no effect on the statement, or the transaction block.

Example 7-38 begins a transaction block within the booktown database.

Example 7-38. Beginning a transaction
```
booktown=# 
BEGIN;
```

**BEGIN**
Any SQL statement made after the BEGIN SQL command will appear to take effect as normal to the user making the modifications. As stated earlier, however, other users connected to the database will be oblivious to the modifications that appear to have been made from within your transaction block until it is committed.

Transaction blocks are closed with the COMMIT SQL command, which may be followed by either of the optional noise terms WORK or TRANSACTION. Example 7-39 uses the COMMIT SQL command to synchronize the database system with the result of an UPDATE statement.

Example 7-39. Committing a transaction
```
booktown=# 
BEGIN;

BEGIN
booktown=# 
UPDATE subjects SET location = NULL

booktown-# 
                WHERE id = 12;

UPDATE 1
booktown=# 
SELECT location FROM subjects WHERE id = 12;

 location
----------

(1 row)

booktown=# 
COMMIT;
```

**COMMIT**
Again, even though the SELECT statement immediately reflects the result of the UPDATE statement in Example 7-39, other users connected to the same database will not be aware of that modification until after the COMMIT statement is executed.

To roll back a transaction, the ROLLBACK SQL command is used. Again, either of the optional noise terms WORK or TRANSACTION may follow the ROLLBACK command.

Example 7-40 begins a transaction block, makes a modification to the subjects table, and verifies the modification within the block. The transaction is then rolled back, returning the subjects table to the state that it was in before the transaction block began.

Example 7-40. Rolling back a transaction
```
booktown=# 
BEGIN;

BEGIN
booktown=# 
SELECT * FROM subjects WHERE id = 12;

 id | subject  | location
----+----------+----------
 12 | Religion |
(1 row)

booktown=# 
UPDATE subjects SET location = 'Sunset Dr'

booktown-# 
                WHERE id = 12;

UPDATE 1
booktown=# 
SELECT * FROM subjects WHERE id = 12;

 id | subject  | location
----+----------+-----------
 12 | Religion | Sunset Dr
(1 row)

booktown=# 
ROLLBACK;

ROLLBACK
booktown=# 
SELECT * FROM subjects WHERE id = 12;

 id | subject  | location
----+----------+----------
 12 | Religion |
(1 row)
```

PostgreSQL is very strict about errors in SQL statements inside of transaction blocks. Even an innocuous parse error, such as that shown in Example 7-41, will cause the transaction to enter into the ABORT STATE. This means that no further statements may be executed until either the COMMIT or ROLLBACK command is used to end the transaction block.

Example 7-41. Recovering from the abort state
```
booktown=# 
BEGIN;

BEGIN
booktown=# 
SELECT * FROM;

ERROR:  parser: parse error at or near ";"
booktown=# 
SELECT * FROM books;

NOTICE:  current transaction is aborted, queries ignored until end of transaction block
*ABORT STATE*
booktown=# 
COMMIT;
```