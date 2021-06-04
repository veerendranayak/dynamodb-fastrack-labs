+++
title = "Step 1 - Design the primary key"
chapter = true
weight = 1
pre = "<b></b>"
+++
Let’s consider the different entities, as suggested in the preceding introduction. In the game, we have the following entities:

>**User**

>**Game**

>**UserGameMapping**

A `UserGameMapping` is a record that indicates a `user` joined a game. There is a many-to-many relationship between `User` and `Game`.

Having a many-to-many mapping is usually an indication that you want to satisfy two Query patterns, and this game is no exception. We have an access pattern that needs to find all users that have joined a game as well as another pattern to find all games that a user has played.

If your data model has multiple entities with relationships among them, you generally use a composite primary key with both HASH and RANGE values. The composite primary key gives us the Query ability on the HASH key to satisfy one of the query patterns we need. In the DynamoDB documentation, the partition key is called HASH and the sort key is called RANGE, and in this guide we use the API terminology interchangeably and especially when we discuss the code or DynamoDB JSON wire protocol format.

The other two data entities— `User` and `Game` — don’t have a natural property for the RANGE value because the access patterns on a `User` or `Game` are a key-value lookup. Because a RANGE value is required, we can provide a filler value for the RANGE key.

With this in mind, let’s use the following pattern for HASH and RANGE values for each entity type.

| Entity      | HASH        |RANGE  |
| ----------- | ----------- | --------- |
| User      | USER#\<USERNAME>       | #METADATA#\<USERNAME> |  
| Game   | GAME#\<GAME_ID>       | #METADATA#\<GAME_ID> |
| UserGameMapping   | GAME#\<GAME_ID>       | USER#\<USERNAME> |


Let’s walk through the preceding table.

For the User entity, the HASH value is USER#\<USERNAME>. Notice that we’re using a prefix to identify the entity and prevent any possible collisions across entity types.

For the RANGE value on the User entity, we’re using a static prefix of #METADATA# followed by the USERNAME value. For the RANGE value, it’s important that we have a value that is known, such as the USERNAME. This allows for single-item actions such as GetItem, PutItem, and DeleteItem.

However, we also want a RANGE value with different values across different User entities to enable even partitioning if we use this column as a HASH key for an index. For that reason, we append the USERNAME.

The Game entity has a primary key design that is similar to the User entity’s design. It uses a different prefix (GAME#) and a GAME_ID instead of a USERNAME, but the principles are the same.

Finally, the UserGameMapping uses the same HASH key as the Game entity. This allows us to fetch not only the metadata for a Game but also all the users in a Game in a single query. We then use the User entity for the RANGE key on the UserGameMapping to identify which user has joined a specific game.

In the next step, we create a table with this primary key design.
