+++
title = "Step 3 - Consider pregame access patterns"
chapter = true
weight = 3
pre = "<b></b>"
+++

Our game is a battle royale game. Players can create a game at a particular map, and other players can join the game. When 50 players have joined the game, the game starts and no additional players can join.

When searching for games to join, players may want to play a particular map. Other players won’t care about the map and will want to browse open games across all maps.

Based on this information, we have the following seven access patterns:

  - Create game (Write)
  - Find open games (Read)
  - Find open games by map (Read)
  - View game (Read)
  - View users in game (Read)
  - Join game for a user (Write)
  - Start game (Write)
