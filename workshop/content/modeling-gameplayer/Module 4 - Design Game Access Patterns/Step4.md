+++
title = "Step 4 - Scan the sparse secondary index"
chapter = true
weight = 4
pre = "<b></b>"
+++

In the previous step, we saw how to find games for a particular map. Some players may prefer to play a specific map, so this is useful. Other players may be willing to play a game at any map. In this section, we show how to find any open game in the application, regardless of the type of map. To do this, we use the Scan API.

In general, you do not want to design your table to use the DynamoDB Scan operation because DynamoDB is built for surgical queries that grab the exact entities you need. A Scan operation grabs a random collection of entities across your table, so finding the entities you need can require multiple round trips to the database.

However, sometimes Scan can be useful. In our situation, we have a sparse secondary index, meaning that our index shouldn’t have that many entities in it. In addition, the index includes only those games that are open, and that is exactly what we need.

For this use case, Scan works great. Let’s see how it works. In the code you downloaded, a `find_open_games.py` file is in the `application/` directory. The contents of the file follow.

````
import boto3

from entities import Game

dynamodb = boto3.client('dynamodb')

def find_open_games():
    resp = dynamodb.scan(
        TableName='battle-royale',
        IndexName="OpenGamesIndex",
    )

    games = [Game(item) for item in resp['Items']]

    return games

games = find_open_games()
print("Open games:")
for game in games:
    print(game)

````
This code is similar to the code in the previous step. However, rather than using the query() method on the DynamoDB client, we use the scan() method. Because we’re using scan(), we don’t need to specify anything about the key conditions like we did with query(). We’re just having DynamoDB return a bunch of items in no specific order.

Run the script with the following command in your terminal.

````
python application/find_open_games.py
````
Your terminal should print a list of nine games that are open across a variety of maps.

````
Open games:
Game<c6f38a6a-d1c5-4bdf-8468-24692ccc4646 -- Urban Underground>
Game<d06af94a-2363-441d-a69b-49e3f85e748a -- Dirty Desert>
Game<873aaf13-0847-4661-ba26-21e0c66ebe64 -- Dirty Desert>
Game<fe89e561-8a93-4e08-84d8-efa88bef383d -- Dirty Desert>
Game<248dd9ef-6b17-42f0-9567-2cbd3dd63174 -- Juicy Jungle>
Game<14c7f97e-8354-4ddf-985f-074970818215 -- Green Grasslands>
Game<3d4285f0-e52b-401a-a59b-112b38c4a26b -- Green Grasslands>
Game<683680f0-02b0-4e5e-a36a-be4e00fc93f3 -- Green Grasslands>
Game<0ab37cf1-fc60-4d93-b72b-89335f759581 -- Green Grasslands>
````



In this step, we saw how using the Scan operation can be the right choice in specific circumstances. We used Scan to grab an assortment of entities from our sparse secondary index to show open games to players.

In the next steps, we satisfy two access patterns:

* Join game for a user (Write)
* Start game (Write)

DynamoDB transactions
To satisfy the “Join game for a user” access pattern in the following steps, we’re going to use DynamoDB transactions. Transactions are popular in relational systems for operations that affect multiple data elements at once. For example, imagine you are running a bank. One customer, Alejandra, transfers $100 to another customer, Ana. When recording this transaction, you would use a transaction to make sure the changes are applied to the balances of both customers rather than just one.

DynamoDB transactions make it easier to build applications that alter multiple items as part of a single operation. With transactions, you can operate on up to 10 items as part of a single transaction request.

In a TransactWriteItem API call, you can use the following operations:

    Put: For inserting or overwriting an item.
    Update: For updating an existing item.
    Delete: For removing an item.
    ConditionCheck: For asserting a condition on an existing item without altering the item.

In the next step, we use a DynamoDB transaction when adding new users to a game while preventing the game from becoming overfilled.
