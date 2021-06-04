+++
title = "Step 2 - Create a sparse secondary index"
chapter = true
weight = 2
pre = "<b></b>"
+++

In this step, we create the sparse secondary index for open games (games that are not full already).

Creating a secondary index is similar to creating a table. In the code you downloaded, you will find a script file in the `scripts/` directory named `add_secondary_index.py`. The contents of that file are the following.
````
import boto3

dynamodb = boto3.client('dynamodb')

try:
    dynamodb.update_table(
        TableName='battle-royale',
        AttributeDefinitions=[
            {
                "AttributeName": "map",
                "AttributeType": "S"
            },
            {
                "AttributeName": "open_timestamp",
                "AttributeType": "S"
            }
        ],
        GlobalSecondaryIndexUpdates=[
            {
                "Create": {
                    "IndexName": "OpenGamesIndex",
                    "KeySchema": [
                        {
                            "AttributeName": "map",
                            "KeyType": "HASH"
                        },
                        {
                            "AttributeName": "open_timestamp",
                            "KeyType": "RANGE"
                        }
                    ],
                    "Projection": {
                        "ProjectionType": "ALL"
                    }
                }
            }
        ],
    )
    print("Table updated successfully.")
except Exception as e:
    print("Could not update table. Error:")
    print(e)

````

Whenever attributes are used in a primary key for a table or secondary index, they must be defined in AttributeDefinitions. Then, we Create a new secondary index in the GlobalSecondaryIndexUpdates property. For this secondary index, we specify the index name, the schema of the primary key and the attributes we want to project.

Note that we did not have to specify that our secondary index is intended to be used as a sparse index. That is purely a function of the data you put in. If you write items to your table that do not have the attributes for your secondary indexes, they will not be included in your secondary index.

Create your secondary index by running the following command.
````
python scripts/add_secondary_index.py
````

Wait until the global secondary index has been created. This takes approximately 5 minutes.

Check output of the following command. If “IndexStatus”: is “CREATING” you will have to wait until “IndexStatus” is “ACTIVE” before continuing to the next step.
````
aws dynamodb describe-table --table-name battle-royale --query "Table.GlobalSecondaryIndexes[].IndexStatus"
````

In the next step, we use the sparse index to find open games by map.
