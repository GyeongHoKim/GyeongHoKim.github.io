---
title: "[DB/SQL] How to create tables in mysql"
excerpt: "table in mysql and primary key"
last_modified_at: 2021-02-05T12:49:00+09:00
categories: DB
tag: ["DB", "mysql"]
toc: true
toc_sticky: true
author_profile: false
---

# MySQL CREATE TABLE

``` sql
CREATE TABLE t (
	id INT NOT NULL AUTO_INCREMENT,
	name VARCHAR NOT NULL,
	price INT DEFAULT 0,
	PRIMARY KEY(id)
);
```

this code create a new table with three columns

## PRIMARY KEY

primary key is a field in a table which uniquely identifies each row/record in a database table. It cannot have NULL and must contain **unique** value.

The end