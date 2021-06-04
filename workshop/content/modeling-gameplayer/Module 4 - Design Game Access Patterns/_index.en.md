+++
title = "Design Game Access Patterns"
chapter = true
weight = 5
description = "Use a global secondary index to find open games"
pre = "<b>Module 4: </b>"
+++

One of the biggest adjustments for users who are new to DynamoDB and NoSQL is how to model data to filter across an entire dataset. For example, in our game, we need to find game sessions with open spots so that we can show users which game session they can join.

In a relational database, you would write some SQL to query the data.

````
SELECT * FROM games
	WHERE status = “OPEN”
````
DynamoDB can filter results on a Query or Scan operation, but DynamoDB doesn’t work like a relational database. A DynamoDB filter applies after the initial items that match the Query or Scan operation have been retrieved. The filter reduces the size of the payload sent from the DynamoDB service, but the number of items retrieved initially is subject to the DynamoDB size limits.

Fortunately, there are a number of ways you can allow filtered queries against your dataset in DynamoDB. To provide efficient filters on your DynamoDB table, you need to plan the filters into your table’s data model from the beginning. Remember the lesson we learned in the second module of this lab: Consider your access patterns, and then design your table.

In the following steps, we use a global secondary index to find open games. Specifically, we will use the sparse index technique to handle this access pattern.
