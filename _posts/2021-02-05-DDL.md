---
title: "[DB/SQL] DDL"
excerpt: "CREATE, ALTER, DROP, TRUNCATE"
last_modified_at: 2021-02-05T12:49:00+09:00
categories: DB
tag: ["DB", "mysql"]
toc: true
toc_sticky: true
author_profile: false
---

# CREATE command

``` sql
CREATE DATABASE SOMETHING
```

I made SOMETHING database.

## create tables

I should **choice** database to create tables. like this.

``` sql
USE SOMETHING

/* when you want to know what the current using database is:
SELECT DATABASE();
*/
```

To make a table,

``` sql
CREATE TABLE table_name(
	'field_name' data_type attributes,
	~
);
```

# ALTER command

ALTER command is used when you want to fix the entity(add field or set limits on a table).

## add field

``` sql
ALTER TABLE table_name ADD field_name data_type;

/*after this, use DESC to check lists of table fields.
DESC table_name;
*/
```

if you use AFTER keyword, you can add the field next to certain field you want.
with FIRST keyword, you add the field at the first of the table.

## modify field

To change name of field, use **ALTER** and **CHANGE keyword** like this

``` sql
ALTER TABLE table_name CHANGE original_name new_name original_data_type;
```

To modify data type of certain field, use **MODIFY keyword**.

``` sql
ALTER TABLE table_name MODIFY field_name new_data_type;
```

To delete certain field, use **DROP keyword**.

``` sql
ALTER TABLE table_name DROP field_name;
```
To change table name, use **RENAME keyword**.

``` sql
ALTER TABLE table_name RENAME new_table_name;
```

# DROP command

Drop command is used when you delete certain entity. You can also use DROP as keyword with ALTER.

``` sql
DROP TABLE table_name;
```

# TRUNCATE command

TRUNCATE command delete all of records of certain tables. It keeps fields.

``` sql
TRUNCATE TABLE table_name;
```

The end