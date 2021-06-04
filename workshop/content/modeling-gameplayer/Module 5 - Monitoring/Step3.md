+++
title = "Step 3 - Download the supporting code"
chapter = true
weight = 3
pre = "<b></b>"
+++

In this lab, you use Python scripts to interact with the DynamoDB API. Run the following commands in your AWS Cloud9 terminal to download and unpack this lab’s code..

````bash
cd ~/environment
curl -sL https://s3.amazonaws.com/ddb-labs/battle-royale.tar | tar -xv
````
You should see two directories in the AWS Cloud9 file explorer:

- application: The application directory contains example code for reading and writing data in your table. This code is similar to code you would have in your real game.
- scripts: The scripts directory contains administrator-level scripts, such as for creating a table, adding a secondary index, or deleting a table.


### Conclusion

In this module, you learned about the example application you build in this lab. You also set up an AWS account and configured an AWS Cloud9 instance.

You are now ready to start the lab. With DynamoDB, it is important to plan your data model up front so that you have fast, consistent performance in your application. In the next module, you learn about planning your data model.
