+++
title = "Step 1 - Model a sparse secondary index"
chapter = true
weight = 1
pre = "<b></b>"
+++

Secondary indexes are crucial data modeling tools in DynamoDB. They allow you to reshape your data to allow for alternate query patterns. To create a secondary index, you specify the primary key of the index, just like when you previously created a table. Note that the primary key for a global secondary index does not have to be unique for each item. DynamoDB then copies items into the index based on the attributes specified, and you can query it just like you do the table.

Using sparse secondary indexes is an advanced strategy in DynamoDB. With secondary indexes, DynamoDB copies items from the original table only if they have the elements of the primary key in the secondary index. Items that don’t have the primary key elements are not copied, which is why these secondary indexes are called “sparse.”

Let’s see how this plays out for us. You might remember that we have two access patterns for finding open games:

* Find open games (Read)
* Find open games by map (Read)

We can create a secondary index using a composite primary key where the HASH key is the map attribute for the game and the RANGE key is the open_timestamp attribute for the game, indicating the time the game was opened.

The important part for us is that when a game becomes full, the open_timestamp attribute is deleted. When the attribute is deleted, the filled game is removed from the secondary index because it doesn’t have a value for the RANGE key attribute. This is what keeps our index sparse: It includes only open games that have the open_timestamp attribute.

In the next step, we create the secondary index.
