+++
title = "Step 8 - Retrieve games for a user"
chapter = true
weight = 8
pre = "<b></b>"
+++

Now that we’ve created our inverted index, let’s use it to retrieve the Game entities played by a User. To handle this, we need to query the inverted index with the User whose Game entities we want to see.

In the code you downloaded, a `find_games_for_user.py` script is in the `application/` directory. The contents of the file are as follows.
````
import boto3

from entities import UserGameMapping

dynamodb = boto3.client('dynamodb')

USERNAME = "carrpatrick"


def find_games_for_user(username):
    try:
        resp = dynamodb.query(
            TableName='battle-royale',
            IndexName='InvertedIndex',
            KeyConditionExpression="SK = :sk",
            ExpressionAttributeValues={
                ":sk": { "S": "USER#{}".format(username) }
            },
            ScanIndexForward=True
        )
    except Exception as e:
        print('Index is still backfilling. Please try again in a moment.')
        return None

    return [UserGameMapping(item) for item in resp['Items']]

games = find_games_for_user(USERNAME)

if games:
    print("Games played by {}:".format(USERNAME))
    for game in games:
        print(game)
````
In this script, we have a function called find_games_for_user() that is similar to a function you would have in your game. This function takes a user name and returns all the games played by the given user.

Run the script in your terminal with the following command.
````
python application/find_games_for_user.py
````

The script should print all of the games played by the user carrpatrick.
````
Games played by carrpatrick:
UserGameMapping<25cec5bf-e498-483e-9a00-a5f93b9ea7c7 -- carrpatrick -- SILVER>
UserGameMapping<c6f38a6a-d1c5-4bdf-8468-24692ccc4646 -- carrpatrick>
UserGameMapping<c9c3917e-30f3-4ba4-82c4-2e9a0e4d1cfd -- carrpatrick>
````


### Conclusion


In this module, we added a secondary index to the table. This satisfied two additional access patterns:

* Find open games by map (Read)
* Find open games (Read)

To accomplish this, we used a sparse index that included only the games that were still open for additional players. We then used both the Query and Scan APIs against the index to find open games.

Also, we saw how to satisfy two advanced write operations in the application. First, we used DynamoDB transactions when a user joined a game. With transactions, we handled a complex conditional write across multiple entities in a single request.

Second, we implemented the function for a creator of a game to start a game when it’s ready. In this access pattern, we had an update operation that required checking the value of three attributes and updating two attributes. You can express this complex logic in a single request through the power of condition expressions and update expressions.

Thirds, we satisfied the final access pattern by retrieving all Game entities played by a User. To handle this access pattern, we created a secondary index using the inverted index pattern to allow querying on the other side of the many-to-many relationship between User entities and Game entities.

We satisfied the following access patterns in our game:


* Create user profile (Write)
* Update user profile (Write)
* Get user profile (Read)
* Create game (Write)
* Find open games (Read)
* View game (Read)
* Join game for a user (Write)
* Start game (Write)
* Update game for a user (Write)
* Update game (Write)
* Find games for user (Read)

The strategies we used to satisfy these patterns include:

* A single-table design that combines multiple entity types in one table.
* A composite primary key that allows for many-to-many relationships.
* A sparse secondary index to filter on one of the fields.
* DynamoDB transactions to handle complex write patterns across multiple entities.
* An inverted index to allow reverse lookups on the many-to-many entity.
