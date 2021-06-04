+++
title = "Step 1 - DynamoDB Metrics"
chapter = true
weight = 1
pre = "<b></b>"
+++

In this  module, you will see how DynamoDB on-demand mode is able scale up capacity with increase in traffic. Also, we will look at Cloudwatch console to review DynamoDB metrics.

1. Run following code to create a data file with 200K items

````
hexdump -v -e '5/1 "%02x""\n"' /dev/urandom |  awk -v OFS='\t' '  { print """{\"PK\":\"GAME#3a816c56-53e9-4d42-a76d-fa4af24b9842#"int(NR/5)"\",\"SK\":\"USER#"$0"\",\"username\":\""$0"\"}" }' | head -n 200000 > ./scripts/game_200k_items.json
````

2. Run following code to load file in DynamoDB table. Expected run time 120 secs.
````
time python ./scripts/bulk_load_table.py ./scripts/game_200k_items.json
````

3. Go Amazon DynamoDB console and review following cloudwatch metrics.



4. As you can see, we did not change capacity to meet this sudden increase. For tables using on-demand mode, DynamoDB instantly accommodates customers’ workloads as they ramp up or down to any previously observed traffic level. If the level of traffic hits a new peak, DynamoDB adapts rapidly to accommodate the workload.
With DynamoDB on-demand you pay only for what you use.

DynamoDB on-demand is useful if your application traffic is difficult to predict and control, your workload has large spikes of short duration, or if your average table utilization is well below the peak. For example:

* New applications, or applications whose database workload is complex to forecast
* Developers working on serverless stacks with pay-per-use pricing
* SaaS provider and independent software vendors (ISVs) who want the simplicity and resource isolation of deploying a table per subscriber
