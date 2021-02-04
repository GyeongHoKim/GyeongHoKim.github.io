---
title: "[DB/SQL] DB environment and DB terms"
excerpt: "mysql server and client & terms for RDBMS"
last_modified_at: 2021-02-04T20:23:00+09:00
categories: DB
tag: ["DB", "mysql"]
toc: true
toc_sticky: true
author_profile: false
---

# What is DataBase?

> A database is a collection of information that is organized so that it can be easily accessed, managed and updated. Computer databases typically contain aggregations of data records or files, containing information about sales transactions or interactions with specific customers.  
> In a relational database, digital information about a specific customer is organized into rows, columns and tables which are indexed to make it easier to find relevant information through SQL or NoSQL queries. In contrast, a graph database uses nodes and edges to define relationships between data entries and queries require a special semantic search syntax.  As of this writing, SPARQL is the only semantic query language that is approved by the World Wide Web Consortium (W3C).  
> *from https://searchsqlserver.techtarget.com/definition/database*  

There are many type of DB and SQL, but I am going to learn RDBMS and MySQL.

# Settings

I used **GCP SQL instance**

1. make VM instance and DB instance at GCP
2. make VM instance open for 3306 port(access for mysql-server)
3. install mysql-client at VM instance
4. `$ mysql -h 35.239.166.192 -P 3306 -u root -p`

# Schema

* Database can have more than one Schema.  
* Schema can have objects like Table, Data Type, Function, Operator.
* No conflict although different Schema have the same object name.
* Schemas are not diveded rigidly

you can group objects by logically.

# Table

basic data storage units consisting of row(record, tuple) and column(field, item).

![table](https://2.bp.blogspot.com/-vnLOqePAUso/WsNiTU6dKyI/AAAAAAAAGDE/ZMNQTX-x3Ws8MTiDV5motp0ivXiVMB5ZACLcBGAs/s1600/table.png)

The end