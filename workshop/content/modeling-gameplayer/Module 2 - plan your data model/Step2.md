+++
title = "Step 2 - Consider user profile access patterns"
chapter = true
weight = 2
pre = "<b></b>"
+++
The users of our game need to create user profiles. These profiles include data such as a user name, avatar, game statistics, and other information about each user. The game displays these user profiles when a user logs in. Other users can view the profile of a user to review their game statistics and other details.

As a user plays games, the game statistics are updated to reflect the number of games the user has played, the number of games the user has won, and the number of kills by the user.

Based on this information, we have three access patterns:
* Create user profile (Write)
* Update user profile (Write)
* Get user profile (Read)
