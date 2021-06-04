+++
title = "Step 3 - Bulk-load data into the table"
chapter = true
weight = 3
pre = "<b></b>"
+++
In this step, we bulk-load some data into the DynamoDB we created in the preceding step. This means that in succeeding steps, we will have sample data to use.

In the `scripts/` directory, you will find a file called `items.json`. This file contains 835 example items that were randomly generated for this lab. These items include `User`, `Game`, and `UserGameMapping` entities. Open the file if you want to see some of the example items.

The `scripts/` directory also has a file called `bulk_load_table.py` that reads the items in the `items.json` file and bulk-writes them to the DynamoDB table. That file’s contents follow.

````
import json

import boto3

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('battle-royale')

items = []

with open('scripts/items.json', 'r') as f:
    for row in f:
        items.append(json.loads(row))

with table.batch_writer() as batch:
    for item in items:
        batch.put_item(Item=item)
````
In this script, rather than using the low-level client in boto3, we use a higher-level Resource object. Resource objects provide an easier interface for using the AWS APIs. The Resource object is useful in this situation because it batches our requests. The BatchWriteItem operation accepts as many as 25 items in a single request. The Resource object handles that batching for us rather than making us divide our data into requests of 25 or fewer items.

Run the **bulk_load_table.py** script and load your table with data by running the following command in the terminal.

````
python scripts/bulk_load_table.py scripts/items.json
````
You can ensure that all your data was loaded into the table by running a Scan operation and getting the count.

````
aws dynamodb scan \
 --table-name battle-royale \
 --select COUNT
````

This should display the following results.

````
{
    "Count": 835,
    "ScannedCount": 835,
    "ConsumedCapacity": null
}
````
You should see a Count of 835, indicating that all of your items were loaded successfully.

In the next step, we show how to retrieve multiple entity types in a single request, which can reduce the total network requests you make in your application and enhance application performance.
