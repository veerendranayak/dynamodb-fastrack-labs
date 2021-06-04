+++
title = "Core Usage: User Profiles and Games"
chapter = true
weight = 4
description = "Design the primary key for the DynamoDB table and enable the core access patterns"
pre = "<b>Module 3: </b>"
+++


In the previous module, we defined the game application’s access patterns. In this module, we design the primary key for the DynamoDB table and enable the core access patterns.

When designing the primary key for a DynamoDB table, keep the following best practices in mind:

  - Start with the different entities in your table. If you are storing multiple different types of data in a single table—such as employees, departments, customers, and orders—be sure your primary key has a way to distinctly identify each entity and enable core actions on individual items.
  - Use prefixes to distinguish between entity types. Using prefixes to distinguish between entity types can prevent collisions and assist in querying. For example, if you have both customers and employees in the same table, the primary key for a customer could be CUSTOMER#, and the primary key for an employee could be EMPLOYEE#.
  - Focus on single-item actions first, and then add multiple-item actions if possible. For a primary key, it’s important that you can satisfy the read and write options on a single item by using the single-item APIs: GetItem, PutItem, UpdateItem, and DeleteItem. You may also be able to satisfy your multiple-item read patterns with the primary key by using Query. If not, you can add a secondary index to handle the Query use cases.

With these best practices in mind, let’s design the primary key for the game application’s table and perform some basic actions.
