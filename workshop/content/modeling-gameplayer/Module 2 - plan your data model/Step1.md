+++
title = "Step 1 - Build your entity-relationship diagram"
chapter = true
weight = 1
pre = "<b></b>"
+++

The first step of any data modeling exercise is to build a diagram to show the entities in your application and how they relate to each other.

In our application, we have the following entities:

- `User`
- `Game`
- `UserGameMapping`

A `User` entity represents a user in our application. A user can create multiple Game entities, and the creator of a game will determine which map is played and when the game starts. A User can create multiple Game entities, so there is a one-to-many relationship between Users and Games.

Finally, a `Game` contains multiple Users and a User can play in multiple different Games over time. Thus, there is a many-to-many relationship between Users and Games. We can represent this relationship with the UserGameMapping entity.

With these entities and relationships in mind, our entity-relationship diagram is shown below.

![Open the ER](/images/ER.png)
