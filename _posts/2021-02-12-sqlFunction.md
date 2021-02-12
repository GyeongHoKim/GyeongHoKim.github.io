---
title: "[DB/SQL] conditional function in SQL"
excerpt: "IF, IFNULL, NULLIF, CASE WHEN, COALESCE"
last_modified_at: 2021-02-12T21:11:00+09:00
categories: DB
tag: DB
toc: true
toc_sticky: true
author_profile: false
---

# sort of Functions

There are two sort of Functions, Internal Function and User-defined Function.
Or, Single Row Function adn Multi Row Function.

# conditional function

``` sql
IF(<statement>, <value if statement is true>, <value if statement is false>)
```

You can use IF function like this,

``` sql
SELECT *, IF(<field name>=<certain field name>) AS <alias name> FROM <table name>;
```

## IFNULL and NULLIF

``` sql
IFNULL(<value1>, <value2>)
```

if value1 is NULL, it returns value2. if value1 is not NULL, then returns value1.

``` sql
NULLIF(<value1>, <value2>)
```

if value1 and value2 are same, it returns NULL, or it returns value1.

## CASE ... WHEN

``` sql
CASE <column name>
WHEN <value1> THEN <return1>
WHEN <value2> THEN <return2>
...
ELSE <etc return>
END
```

you can enter conditional statement in \<value>

## COALESE

``` sql
COALESCE (<value1>, <value2>, ... , <valueN>)
```

if value1 is NULL, return value2.
if value1 and value2 both NULL, return value3
.  
.  
.  
if value1 ~ value(N-1) are NULL, return valueN

The End