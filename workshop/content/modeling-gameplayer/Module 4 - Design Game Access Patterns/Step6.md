+++
title = "Step 6 - Start a game"
chapter = true
weight = 6
pre = "<b></b>"
+++

As soon as a game has 50 users, the creator of the game can start the game to initiate gameplay. In this step, we show how to handle this access pattern.

When our application backend receives a request to start the game, we check three things:

* The game has 50 people signed up.
* The requesting user is the creator of the game.
* The game has not already started.

We can handle each of these checks in a condition expression in a request to update the game. If all of these checks pass, we need to update our entity in the following ways:

* Remove the open_timestamp attribute so that it does not appear as an open game in the sparse secondary index from the previous module.
* Add a start_time attribute to indicate when the game started.

In the code you downloaded, a `start_game.py` script is in the `application/` directory. The contents of the file are as follows.

````
import datetime

import boto3

from entities import Game

dynamodb = boto3.client('dynamodb')

GAME_ID = "c6f38a6a-d1c5-4bdf-8468-24692ccc4646"
CREATOR = "gstanley"

def start_game(game_id, requesting_user, start_time):
    try:
        resp = dynamodb.update_item(
            TableName='battle-royale',
            Key={
                "PK": { "S": "GAME#{}".format(game_id) },
                "SK": { "S": "#METADATA#{}".format(game_id) }
            },
            UpdateExpression="REMOVE open_timestamp SET start_time = :time",
            ConditionExpression="people = :limit AND creator = :requesting_user AND attribute_not_exists(start_time)",
            ExpressionAttributeValues={
                ":time": { "S": start_time.isoformat() },
                ":limit": { "N": "50" },
                ":requesting_user": { "S": requesting_user }
            },
            ReturnValues="ALL_NEW"
        )
        return Game(resp['Attributes'])
    except Exception as e:
        print('Could not start game')
        return False

game = start_game(GAME_ID, CREATOR, datetime.datetime(2019, 4, 16, 10, 15, 35))

if game:
    print("Started game: {}".format(game))
````


In this script, the start_game function is similar to the function you would have in your application. It takes a game_id, requesting_user, and start_time, and it runs a request to update the Game entity to start the game.

The ConditionExpression parameter in the update_item() call specifies each of the three checks that we listed earlier in this step—the game must have 50 people, the user requesting that the game start must be the creator of the game, and the game cannot have a start_time attribute, which would indicate it already started.

In the UpdateExpression parameter, you can see the changes we want to make to our entity. First we remove the open_timestamp attribute from the entity, and then we set the start_time attribute to the game’s start time.

Run this script in your terminal with the following command.
````
python application/start_game.py
````
You should see output in your terminal indicating that the game was started successfully.
````
Started game: Game<c6f38a6a-d1c5-4bdf-8468-24692ccc4646 -- Urban Underground>
````



Try to run the script a second time in your terminal. This time, you should see an error message that indicates you could not start the game. This is because you have already started the game, so the start_time attribute exists. As a result, the request failed the conditional check on the entity.

**Inverted index pattern**

You might recall that there is a many-to-many relationship between the `Game` entity and the associated `User` entities, and the relationship is represented by a `UserGameMapping` entity.

Often, you want to query both sides of a relationship. With our primary key setup, we can find all the `User` entities in a `Game`. We can enable querying all `Game` entities for a `User` by using an inverted index.

In DynamoDB, an inverted index is a secondary index that is the inverse of your primary key. The RANGE key becomes your HASH key and vice versa. This pattern flips your table and allows you to query on the other side of your many-to-many relationships.

In the following steps, we add an inverted index to the table and show how to use it to retrieve all Game entities for a specific User.
