+++
title = "Step 2 - Create a table"
chapter = true
weight = 2
pre = "<b></b>"
+++
Now that we have designed the primary key, let’s create a table.

The code you downloaded in Step 3 of Module 1 includes a Python script in the `scripts/` directory named `create_table.py`. The Python script’s contents follow.

````
import boto3

dynamodb = boto3.client('dynamodb')

try:
    dynamodb.create_table(
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
        KeySchema=[
            {
                "AttributeName": "PK",
                "KeyType": "HASH"
            },
            {
                "AttributeName": "SK",
                "KeyType": "RANGE"
            }
        ],
        BillingMode='PAY_PER_REQUEST'
    )
    print("Table created successfully.")
except Exception as e:
    print("Could not create table. Error:")
    print(e)
````


The preceding script uses the CreateTable operation using Boto 3, the AWS SDK for Python. The operation declares two attribute definitions, which are typed attributes to be used in the primary key. Though DynamoDB is schemaless, you must declare the names and types of attributes that are used for primary keys. The attributes must be included on every item that is written to the table and thus must be specified as you are creating a table.

Because we’re storing different entities in a single table, we can’t use primary key attribute names such as UserId. The attribute means something different based on the type of entity being stored. For example, the primary key for a user might be its USERNAME, and the primary key for a game might be its GAMEID. Accordingly, we use generic names for the attributes, such as PK (for partition key) and SK (for sort key).

After configuring the attributes in the key schema, we specify the On-demand throughput for the table.

To create the table, run the Python script with the following command.

````
python scripts/create_table.py
````

The script should return this message: “Table created successfully.”

In the next step, we bulk-load some example data into the table.
