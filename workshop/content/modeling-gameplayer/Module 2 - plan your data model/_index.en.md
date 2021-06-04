+++
title = "Plan Your Data Model"
chapter = true
weight = 3
description = "Build your entity-relationship diagram and map out our access patterns up front"
pre = "<b>Module 2: </b>"
+++


Data modeling is the process of designing how an application stores data in a given database.

NoSQL (nonrelational) databases are designed for speed and scale. Though the performance of your relational database may degrade as you scale up, horizontally scaling databases such as DynamoDB provides consistent performance at any scale.

Achieving best results with a NoSQL database such as DynamoDB requires a shift in thinking from the typical relational database. Use the following best practices when modeling data with DynamoDB. Use the following best practices when modeling data with DynamoDB.

1. **Focus on access patterns**:
  When doing any kind of data modeling, you will start with an entity-relationship diagram that describes the different objects (or entities) in your application and how they are connected (or the relationships between your entities).  
  In relational databases, you will put your entities directly into tables and specify relationships using foreign keys
  In DynamoDB, you think about access patterns before modeling your table. You first ask how you will access your data, and then model your data in the shape it will be accessed.

2. **Optimize for the number of requests to DynamoDB**
  After you have application’s access pattern needs, you are ready to design your table. You should design your table to minimize the number of requests to DynamoDB for each access pattern. Ideally, each access pattern should require only a single request to DynamoDB because network requests are slow, and this limits the number of network requests you will make in your application.

3. **Don’t fake a relational model**
  People new to DynamoDB often try to implement a relational model on top of DynamoDB. If you try to do this, you will lose most of the benefits of DynamoDB. The most common anti-patterns (ineffective responses to recurring problems) that people try with DynamoDB are:
  - **Normalization**: In a relational database, you normalize your data to reduce data redundancy and storage space, and then use joins to combine multiple different tables. However, joins at scale are slow and expensive. DynamoDB does not allow for joins because they slow down as your table grows.
  - **One data type per table**: Your DynamoDB table will often include different types of data in a single table. In our example, we have User, Game, and UserGameMapping entities in a single table. In a relational database, this would be modeled as three different tables.
  - **Too many secondary indexes**: People often try to create a secondary index for each additional access pattern they need. DynamoDB is schemaless, and this applies to your indexes, too. Use the flexibility in your attributes to reuse a single secondary index across multiple data types in your table. This is called [index overloading](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-gsi-overloading.html).

In the steps below, we will build our entity-relationship diagram and map out our access patterns up front. These should always be your first steps when using DynamoDB. Then, in the modules that follow, we implement these access patterns in the table design.
