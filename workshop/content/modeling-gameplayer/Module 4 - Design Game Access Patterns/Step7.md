+++
title = "Step 7 - Add an inverted index"
chapter = true
weight = 7
pre = "<b></b>"
+++
In this step, we add an inverted index to the table. An inverted index is created like any other secondary index.

In the code you downloaded, a `add_inverted_index.py` script is in the `scripts/` directory. This Python script adds an inverted index to your table.

The contents of that file are as follows.
````
import boto3

dynamodb = boto3.client('dynamodb')

try:
    dynamodb.update_table(
        TableName='battle-royale',
        AttributeDefinitions=[
            {
                "AttributeName": "PK",
                "AttributeType": "S"
            },
            {
                "AttributeName": "SK",
                "AttributeType": "S"
            }
        ],
        GlobalSecondaryIndexUpdates=[
            {
                "Create": {
                    "IndexName": "InvertedIndex",
                    "KeySchema": [
                        {
                            "AttributeName": "SK",
                            "KeyType": "HASH"
                        },
                        {
                            "AttributeName": "PK",
                            "KeyType": "RANGE"
                        }
                    ],
                    "Projection": {
                        "ProjectionType": "ALL"
                    },
                    "ProvisionedThroughput": {
                        "ReadCapacityUnits": 1,
                        "WriteCapacityUnits": 1
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


In this script, we call an update_table() method on our DynamoDB client. In the method, we pass details about the secondary index we want to create, including the key schema for the index, the provisioned throughput, and the attributes to project into the index.

Run the script by typing the following command in your terminal.

````
python scripts/add_inverted_index.py

````
Wait until the global secondary index has been created and is in active status. This takes approximately 5 minutes.

Check output of the following command. 

````
aws dynamodb describe-table --table-name battle-royale --query "Table.GlobalSecondaryIndexes[].IndexStatus"
````
Wait until the new index is ACTIVE before proceeding.

````
[
    "ACTIVE",
    "ACTIVE"
]
````

In the next step, we use our inverted index to retrieve all Game entities for a particular User.
