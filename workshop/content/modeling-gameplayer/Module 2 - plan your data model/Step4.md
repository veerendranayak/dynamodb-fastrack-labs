+++
title = "Step 4 - In-game and post-game access patterns"
chapter = true
weight = 4
pre = "<b></b>"
+++

Finally, let’s consider the access patterns during and after a game.

During the game, players try to defeat other players with the goal of being the last player standing. Our application tracks how many kills each player has during a game as well as the amount of time a player survives in a game. If a player is one of the last three surviving in a game, the player receives a gold, silver, or bronze medal for the game.

Later, players may want to review past games they’ve played or that other players have played.

Based on this information, we have three access patterns:

  - Update game for user (Write)
  - Update game (Write)
  - Find all past games for a user (Read)

  ### Conclusion
  We have now mapped all access patterns for the game. In the following modules, we implement these access patterns by using DynamoDB.

  Note that the planning phase might take a few iterations. Start with a general idea of the access patterns your application needs. Map the primary key, secondary indexes, and attributes in your table. Go back to the beginning and make sure all of your access patterns are satisfied. When you are confident the planning phase is complete, move forward with implementation.
