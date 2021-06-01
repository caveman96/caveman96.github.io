---
title: What is a noSQL DB, and why you should use one.
author: Silesh Chandran
date: 2021-05-30 20:10:00 +0530
categories: [basics, backend]
tags: [backend, java, mongodb, nosql, database]
toc: true
comments: true
---

Most people know whats a relational Database, we learn about it in college. You create tables, insert data, perform joins and run queries with SQL. But this is not your only option if you need a database. Something that's been gaining a lot of traction lately is the use of NoSQL databases. I've unfortunately never had to work with a noSQL database and I am remedying that with this post, by understanding exactly what these bring to the table and why someone would use it.

## What is a noSQL DB?

Quite simply a noSQL database is one that stores data in a non relational format. To understand what noSQL entails, its easier to see what comes with traditional relational databases. 

Relational databases involve tables of fixed columns which are linked to each other via key references. Its clear to see that relational databases are deeply tied down to object oriented modeling techniques, where each class corresponds to a table in the db. The problem with relational databases is that it can be quite inflexible, it doesn't translate well to non object oriented domains and it doesn't play well with distributed systems, both ease as well as performance wise. 

NoSQL databases tackle these issues by providing something that is less tightly coupled and is much more flexible in terms of design (supposedly, we'll find out soon enough). They allow the storage and retrieval of a variety of data types and formats. You get flexibility and better performance at the expense of storage since its not as structured and minimal as a relational DB.

## Types of noSQL databases:

There are more types than the ones mentioned below, but I found these to be the most commonly used. (others include object DBs, tabular DBs etc)

### Document databases
This is what I'm most interested in and what most people think about in context of no SQL databases. Quite simply, these store values in a json format. The values can be of any data type like strings, numbers, booleans or other objects. This allows storage of object details directly, without having to worry about a predetermined structure and being limited to database schema. Horizontal scaling is quite simple as Json objects can grow as per need. MongoDB is an example for this type of noSQL database.
![Desktop View](https://webassets.mongodb.com/_com_assets/cms/Relational_vs_DocumentDB-imgngssl17.png)

### Key-value databases 
These are similar to in memory maps that are available in most programming languages (dict, hashmap etc). They are generally used to cache large values and retrieve them without having to perform large operations. Redis is one such database that I have worked with in the past. Its quite commonly used as an in memory data store, for storing runtime artifacts that needs to be made available to several components of an application. These are also quite frequently used as a cache on top of an existing DB to improve performance.
![Desktop View](https://upload.wikimedia.org/wikipedia/commons/5/5b/KeyValue.PNG)

### Wide-column stores
These store data in tables, rows, and dynamic columns. These are similar to relational databases in structure but with the added flexibility of not being limited to fixed columns per row. They can be thought of as two dimensional key-value databases. Cassandra and Hbase are examples. 

Hbase is built based on google bigTable, and I found its design quite interesting. It feels like a mash up of relational, document and key-value databases. Each row in the table is accessed by a key which can be arbitrary strings. There are fixed column families and these are populated per record/object similar to a relational DB. A column family stores any number of related information, similar to a document store. Number of families are expected to be constant, whereas a family may contain as many columns as needed.
(more info in this [**paper**](https://research.google/pubs/pub27898/)
![Desktop View](https://dv-website.s3.amazonaws.com/uploads/2018/09/wcd-pic1.png)

### Graph databases
These store data in nodes linked by edges, like a typical graph data structure. These are super useful in scenarios where there is a network of interrelated objects and traversing them to find relationships is vital. Neo4j and JanusGraph are examples. These are useful in representing networks like in social media and interconnected devices (ioT) or data from multiple mobile devices.

![Desktop View](https://dist.neo4j.com/wp-content/uploads/20180711200201/twitter-users-graph-database-model-peter-emil-johan.png)

## Why use a noSQL database?

Lets try to deduce why one would want to use a noSQL database rather than setting up their own relational database. These are points that I feel are valid and will be put to the test once I'm able to build something concrete and validate for myself.

1. Faster development:
This seems quite apparent. Since noSQL DBs are more flexible and forgiving in terms of data types and structure, it stands to reason that it should be significantly easier to get started with one. The control of what is stored in the DB will be at the hands of the developer and can adapt to various needs. Too much control is often a bad thing, and I am not sure if this advantage will end up crippling the database in a larger work environment where multiple developers and services are writing to the same database.

2. Flexibility in terms of what can be stored:
Where normal relational databases are often concrete structures and the types of values that can be stored in DB are limited by design, a noSQL DB should pose no such restrictions.

3. Ease of data retrieval:
From what I understand so far, it should be possible to store the data in the exact format that it needs to be read into. This should lead to easier Object–relational mapping and should also allow for the data to be more meaningful as there is not restriction on what should be stored.

4. More options:
There are so many different kinds of noSQL databases, each with its own advantages, that it is likely you will find one that fits your specific need. This saves a lot of time and effort involved in having to retrofit your design to a database that is not suited for it. The CAP theorem provides a good way of deciding what database to go with depending on your requirements.

I am still skeptical about the long term viability and support of these databases for larger applications, but so far it seems like a no brainer for small projects. Sometimes having restrictions is a good thing as it leads to long term integrity and stability.

## Whats next:

Since my goal is to find out how a noSQL database is better than a relational one, trying out a document DB or a wide-column store would be the next logical step. MongoDB Atlas seems like a decent place to start, so I think creating a Java project and exposing some API's to do CRUD operations should give me a decent idea on how it works. Hbase also looks quite useful for some scenarios. So stay tuned for updates on that. Thanks for reading 😁.


 

