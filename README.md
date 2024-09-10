AMAZON AWS INVENTORY MANAGEMENT 
# AMAZON AWS INVENTORY MANAGEMENT : Serverless Architecture on AWS (Guide)

**Introduction:**
Welcome to the guide on setting up a serverless architecture on AWS. This README provides an overview of the steps outlined below and project developed by [K.YUVA SHANKAR](https://www.linkedin.com/in/yuva-shankar-4ba786228?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app).

## Steps
1. **Creating an S3 Bucket**: Set up an S3 bucket to store data files.
2. **Creating a DynamoDB Table**: Establish a DynamoDB table to store inventory data.
3. **Creating an SNS Topic**: Create an SNS topic for sending notifications.
4. **Creating Roles for Lambda Functions**: Set up IAM roles for Lambda functions to access S3, DynamoDB, and SNS.
5. **Creating Lambda Functions**: Develop Lambda functions to read from S3, update DynamoDB, and send SNS notifications.


#Table of Contents

Introduction

Prerequisites

Step 1: Creating an S3 Bucket

Step 2: Creating a DynamoDB Table

Step 3: Creating an SNS Topic

Step 4: Creating Roles for Lambda Functions

Step 5: Developing Lambda Functions

Step 6: Testing the Setup

Step 7: Clean-Up

Optional: Using CloudFormation for Automation

Conclusion

#Introduction

Inventory management is a critical aspect of any business, ensuring that products are tracked, stored, and replenished efficiently. By leveraging AWS's serverless architecture, businesses can automate and streamline these processes, reducing overhead and increasing agility. This guide provides a comprehensive walkthrough on how to set up an inventory management system using AWS services such as S3, DynamoDB, SNS, and Lambda.

#Prerequisites

Before you begin, ensure that you have the following:

An active AWS account with permissions to create and manage S3 buckets, DynamoDB tables, SNS topics, Lambda functions, and IAM roles.
Basic understanding of AWS services, especially S3, DynamoDB, SNS, and Lambda.
Familiarity with the AWS Management Console and AWS CLI (optional).

#Step 1: Creating an S3 Bucket

The first step in setting up your inventory management system is to create an S3 bucket where inventory data files will be stored.

Log in to the AWS Management Console and navigate to the S3 service.
Click on Create bucket.
Name your bucket with a globally unique name, for example, inventory-management-bucket.
Choose the AWS Region where you want to create the bucket. It’s recommended to select a region close to your operations to minimize latency.
Configure additional settings as needed, such as enabling versioning, encryption, and logging. These settings can help you maintain data integrity and security.
Click on Create bucket to finalize the creation.
Your S3 bucket is now ready to store inventory data files, such as CSV files containing information on products, stores, and stock counts.

#Step 2: Creating a DynamoDB Table

Next, you need to set up a DynamoDB table to store and manage the inventory data.

Navigate to the DynamoDB service in the AWS Management Console.
Click on Create table.
Name your table, for example, InventoryTable.
Specify the Primary key. A common approach is to use a composite key:
Partition key: StoreID (String)
Sort key: ProductID (String)
Configure Read/Write Capacity settings:
Choose between Provisioned or On-Demand based on your expected traffic.
Enable Auto Scaling to adjust capacity automatically if you choose Provisioned mode.
Add any additional settings, such as enabling streams or encryption.
Click on Create table to finalize the setup.
Your DynamoDB table is now ready to store and manage inventory records, such as product availability in different stores.

#Step 3: Creating an SNS Topic

To handle notifications, particularly for low inventory alerts, you will set up an SNS (Simple Notification Service) topic.

Navigate to the SNS service in the AWS Management Console.
Click on Create topic.
Choose the Standard type for the topic, which supports multiple subscribers and high-throughput message delivery.
Name your topic, for example, LowInventoryAlerts.
(Optional) Configure access permissions to control who can publish and subscribe to this topic.
Click on Create topic to finalize.
Once the topic is created, you can add subscriptions (such as email or SMS) to receive notifications whenever a low inventory event is triggered.

#Step 4: Creating Roles for Lambda Functions

Lambda functions require permissions to access AWS resources like S3, DynamoDB, and SNS. You'll create an IAM role that grants these permissions.

Go to the IAM (Identity and Access Management) service in the AWS Management Console.
Click on Roles and then Create role.
Select AWS service as the trusted entity and choose Lambda as the service that will use this role.
Attach the necessary policies:
AmazonS3FullAccess to allow Lambda to interact with S3.
AmazonDynamoDBFullAccess to allow Lambda to update the DynamoDB table.
AmazonSNSFullAccess to allow Lambda to publish messages to the SNS topic.
AWSLambdaBasicExecutionRole for CloudWatch logging.
Name the role, e.g., LambdaInventoryManagementRole, and click on Create role.
Your IAM role is now ready to be attached to the Lambda functions you will create in the next step.

#Step 5: Developing Lambda Functions

You will create a series of Lambda functions that will handle different aspects of the inventory management process.

**Function 1: Reading from S3 and Updating DynamoDB**

Go to the Lambda service in the AWS Management Console and click on Create function.
Choose Author from scratch and name your function, e.g., ProcessInventoryFile.
Select Python 3.x or Node.js as the runtime.
Choose the IAM role created earlier (LambdaInventoryManagementRole).
Write the function code to:
Read the CSV file from the S3 bucket.
Parse the file and update the DynamoDB table with the inventory data.
Set up an S3 trigger so this function executes automatically whenever a new file is uploaded to the S3 bucket.
Click on Deploy to save your function.

**Function 2: Sending Low Inventory Alerts via SNS**

Repeat the steps above to create another Lambda function, e.g., SendLowInventoryAlert.
This function will:
Scan the DynamoDB table for products with inventory below a certain threshold.
Send a notification to the SNS topic if low inventory is detected.
Schedule this function using CloudWatch Events to run at regular intervals or trigger it based on specific conditions.

#Step 6: Testing the Setup

Once all components are in place, it's time to test the setup to ensure everything works as expected.

Upload a sample CSV file to the S3 bucket. The file should include columns for StoreID, ProductID, and InventoryCount.
Monitor the execution of the Lambda functions using CloudWatch Logs. Verify that the ProcessInventoryFile function is triggered, updates the DynamoDB table, and that low inventory alerts are sent via the SNS topic if applicable.
Check the DynamoDB table to confirm that the inventory data has been updated correctly.
Verify that SNS notifications have been received by the subscribers.

#Step 7: Clean-Up

After testing, it's important to clean up the resources to avoid incurring unnecessary charges.

Delete the S3 bucket and its contents.
Delete the DynamoDB table from the DynamoDB console.
Delete the SNS topic and its subscriptions.
Delete the Lambda functions from the Lambda console.
Delete the IAM role created for the Lambda functions to remove associated permissions.
Optional: Using CloudFormation for Automation
For more efficient management and deployment, consider creating an AWS CloudFormation template. This template will automate the creation of the S3 bucket, DynamoDB table, SNS topic, Lambda functions, and IAM roles with a single click.

**Steps to Create a CloudFormation Template:**

Define resources in the template file using YAML or JSON.
Specify the necessary permissions and triggers for each service.
Deploy the stack via the AWS Management Console or AWS CLI.
This approach is particularly useful for replicating the setup in different environments or scaling it across multiple regions.

#Conclusion

Implementing a serverless architecture on AWS for inventory management offers a scalable, cost-effective solution that can dynamically respond to your business needs. By using AWS services like S3, DynamoDB, SNS, and Lambda, you can automate and streamline inventory processes, reducing manual effort and improving operational efficiency.

This guide has provided a comprehensive overview of how to set up such an architecture. Whether you are looking to automate inventory management or implement other serverless applications, AWS provides the tools necessary to build

## Testing

1. **Upload a CSV File**: Upload a CSV file with store, product, and count columns to the S3 bucket.
2. **Verify Functionality**: Ensure Lambda functions process the file, update DynamoDB, and send notifications for low inventory.

## Clean Up

1. **Delete Resources**: Remove uploaded objects, S3 bucket, SNS topic, DynamoDB table, Lambda functions, and associated IAM roles.

## Using CloudFormation
You can create a CloudFormation template to create the above infrasture in the click of a button.

#Conclusion

Implementing a serverless architecture on AWS for inventory management offers a scalable, cost-effective solution that can dynamically respond to your business needs. By using AWS services like S3, DynamoDB, SNS, and Lambda, you can automate and streamline inventory processes, reducing manual effort and improving operational efficiency.

This guide has provided a comprehensive overview of how to set up such an architecture. Whether you are looking to automate inventory management or implement other serverless applications, AWS provides the tools necessary to build
---

*Author: [K.YUVA SHANKAR](https://www.linkedin.com/in/yuva-shankar-4ba786228?utm_source=share&utm_campaign=share_via&utm_content=profile&utm_medium=android_app).
