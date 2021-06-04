+++
title = "Step 2 - Setup your AWS Cloud 9"
chapter = true
weight = 2
pre = "<b></b>"
+++

AWS Cloud9 is a cloud-based integrated development environment (IDE) that lets you write, run, and debug code with just a browser.

To set up your AWS Cloud9 development environment:

1. In AWS Management Console, choose Services at the top of the page, and then choose Cloud9 under Developer Tools.
2. Choose Create environment.
3. Type `DynamoDB Battle Royale` in the Name box. Leave the Description box empty.
4. Choose Next step.
5. Leave the Environment settings at their defaults to create a new t2.micro EC2 instance, which will be hibernated after 30 minutes of inactivity.
6. Choose Next step.
7. Review the environment name and settings, and choose Create environment. Your environment will be provisioned and prepared after several minutes.
8. When the environment is ready, your IDE should open with a welcome note.

You should now see your AWS Cloud9 environment. You need to be familiar with the three areas of the AWS Cloud9 console shown in the following screenshot:

- File explorer: On the left side of the IDE, the file explorer shows a list of the files in your directory.
- File editor: On the upper right area of the IDE, the file editor is where you view and edit files that you’ve selected in the file explorer.
- Terminal: On the lower right area of the IDE, this is where you run commands to execute code samples.

![Cloud 9](/images/Cloud9.png)
