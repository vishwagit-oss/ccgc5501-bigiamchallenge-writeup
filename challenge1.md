Big IAM Challenge - Challenge 1: Buckets of Fun
Overview
In Challenge 1, the task was to exploit an AWS S3 bucket with misconfigured IAM policies to retrieve a file, which contained the flag. The IAM policy allowed public access to the files/ folder, which was enough to list and download its contents without credentials.

Steps to Solve the Challenge
Step 1: Analyzing the IAM Policy
The IAM policy granted public access to:

Retrieve objects (s3:GetObject) in the files/ folder.
List the contents of the bucket, specifically with keys beginning with files/.
Step 2: Listing the Objects
Using the AWS CLI, I ran the following command to list the contents of the files/ folder:

aws s3 ls s3://thebigiamchallenge-storage-9979f4b/files/ --no-sign-request
The output showed the flag1.txt file.

Step 3: Downloading the Flag
I downloaded the flag1.txt file using the following command:

aws s3 cp s3://thebigiamchallenge-storage-9979f4b/files/flag1.txt /tmp/ --no-sign-request
Step 4: Retrieving the Flag
I opened the flag1.txt file to reveal the flag, completing the challenge.

Conclusion
This challenge demonstrated the importance of properly configuring IAM policies to prevent unauthorized access. By using AWS CLI and understanding IAM, I was able to exploit the misconfiguration and retrieve the flag.
