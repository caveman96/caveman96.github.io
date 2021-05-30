---
title: What is a noSQL DB, and why you should use one.
author: Silesh Chandran
date: 2021-05-30 20:10:00 +0530
categories: [basics, backend]
tags: [backend, java, mongodb, nosql, database]
toc: true
comments: true
---

I'm sure most people have heard of a noSQL database. I've heard too, but unfortunately have never had the chance to work with one. I will try to remedy that with this post, by understanding what a noSQL DB why one would want to use it instead of a relational database. I've been going through a couple of resources and these are my learnings.

## What is a noSQL DB?

Quite simply a noSQL database is one that stores data in a non relational format. To understand what noSQL entails, its easier to see what comes with traditional relational databases. 

Relational databases involve tables of fixed columns which are linked to each other via key references. The language used to execute queries to fetch data from these tables is SQL. Its clear to see that relational databases are deeply tied down to object oriented modeling techniques, where each class corresponds to a table in the db. The problem with relational databases is that it can be quite inflexible, it doesn't translate well to non object oriented domains and it doesn't play well with distributed systems, both ease as well as performance wise. 

NoSQL databases tackle these issues by providing something that is less tightly coupled and is much more flexible in terms of design (supposedly, we'll find out soon enough). They allow the storage and retrieval of a variety of data types and formats. You get flexibility and better performance at the expense of storage since its not as structured and minimal as a relational DB.

## Types of noSQL databases:

There are more types than the ones mentioned below, but I found these to be the most commonly used. (others include object DBs, tabular DBs etc)

### Document databases
This is what I'm most interested in and what most people think about in context of no SQL databases. Quite simply, these store values in a json format. The values can be of any datatype like strings, numbers, booleans or other objects. This allows storage of object details directly, without having to worry about a predetermined structure and being limited to database schema. Horizontal scaling is quite simple as Json objects can grow as per need. MongoDB is an example for this type of noSQL database.
![Desktop View](https://webassets.mongodb.com/_com_assets/cms/Relational_vs_DocumentDB-imgngssl17.png)

### Key-value databases 
These are similar to in memory maps that are available in most programming languages (dict, hashmap etc). They are generally used to cache large values and retrieve them without having to perform large operations. Redis is one such database that I have worked with in the past. Though my applications have been limited to its use as a layer over the DB itself or as an in memory data store to improve performance.
![Desktop View](https://upload.wikimedia.org/wikipedia/commons/5/5b/KeyValue.PNG)

### Wide-column stores
These store data in tables, rows, and dynamic columns. These are similar to relational databases in structure but with the added flexibility of not being limited to fixed columns per row. They can be thought of as two dimensional key-value databases. Cassandra and Hbase are examples. 

Hbase is built based on google bigTable. (paper here: https://research.google/pubs/pub27898/). The row keys in a table are arbitrary strings, and column keys are grouped into sets called column families. All data stored in a column family is usually same type. Number of families are expected to be constant, whereas a family may contain as many columns as needed.
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

I am still skeptical about the long term viability and support of these databases for larger applications, but so far it seems like a no brainer for small projects. Sometimes having restrictions is a good thing as it leads to long term integrity and stability.

## Whats next:

Since my goal is to find out how a noSQL database is better than a relational one, trying out a document DB or a wide-column store would be the next logical step. MongoDB Atlas seems like a decent place to start, so I think creating a Java project and exposing some API's to do CRUD operations should give me a decent idea on how it works. So stay tuned for updates on that. Thanks for reading 😁.


 

