+++
title = "Step 3 - Query the sparse secondary index"
chapter = true
weight = 3
pre = "<b></b>"
+++

Now that we have configured the secondary index, let’s use it to satisfy some of the access patterns.

To use a secondary index, you have two API calls available: Query and Scan. With Query, you must specify the HASH key, and it returns a targeted result. With Scan, you don’t specify a HASH key, and the operation runs across your entire table. Scans are discouraged in DynamoDB except in specific circumstances because they access every item in your database. If you have a significant amount of data in your table, scanning can take a very long time. In the next step, we show you why Scans can be a powerful tool when used with sparse indexes.

We can use the Query API against the secondary index we created in the previous step to find all open games by map name. The secondary index is partitioned by map name, allowing us to make targeted queries to find open games.

In the code you downloaded, a `find_open_games_by_map.py` file is in the `application/` directory. The contents of this script follow.

````
import boto3

from entities import Game

dynamodb = boto3.client('dynamodb')

def find_open_games_by_map(map_name):
    resp = dynamodb.query(
        TableName='battle-royale',
        IndexName="OpenGamesIndex",
        KeyConditionExpression="#map = :map",
        ExpressionAttributeNames={
            "#map": "map"
        },
        ExpressionAttributeValues={
            ":map": { "S": map_name },
        },
        ScanIndexForward=True
    )

    games = [Game(item) for item in resp['Items']]

    return games

games = find_open_games_by_map("Green Grasslands")
for game in games:
    print(game)

````
In the preceding script, the `find_open_games_by_map` function is similar to a function you would have in your application. The function accepts a map name and makes a query against the `OpenGamesIndex` to find all open games for the map. It then assembles the returned entities into Game objects that can be used in your application.

Execute this script by running the following command in your terminal.

````
python application/find_open_games_by_map.py "Green Grasslands"
````

The terminal will show the following output with four open games for the Green Grasslands map.

````
Open games for Green Grasslands:
Game<14c7f97e-8354-4ddf-985f-074970818215 -- Green Grasslands>
Game<3d4285f0-e52b-401a-a59b-112b38c4a26b -- Green Grasslands>
Game<683680f0-02b0-4e5e-a36a-be4e00fc93f3 -- Green Grasslands>
Game<0ab37cf1-fc60-4d93-b72b-89335f759581 -- Green Grasslands>
````
In the next step, we use the Scan API to scan the sparse secondary index.
