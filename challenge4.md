#  **Big IAM Challenge 4 – Write-up & Explanation**  

##  **Challenge Overview**  
In this challenge, I attempted to **access an Amazon S3 bucket** (`thebigiamchallenge-admin-storage-abf1321`) and retrieve a specific file (`flag-as-admin.txt`).  
The goal was to **understand IAM policies and access control mechanisms** within AWS.

---

##  **Steps Taken and Observations**  

###  **Step 1: Listing Files in the S3 Bucket**  

aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/
 Result:


An error occurred (AccessDenied) when calling the ListObjectsV2 operation: Access Denied
* Key Takeaway:

I do not have permission to list objects in the files/ directory.
This suggests that IAM policies are restricting s3:ListBucket access.
  Step 2: Checking for Public Access to the File

aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt --no-sign-request
* Result:


42 flag-as-admin.txt
- Important Discovery:

The file is publicly readable without authentication (--no-sign-request bypasses credentials).
This suggests a bucket policy allows unauthenticated users (Principal: *) to access objects under certain conditions.
🔹 Step 3: Retrieving the File Content

aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt - --no-sign-request
- Flag Output:
{wiz:principal-arn-is-not-what-you-think}
- Key Clue:
The phrase principal-arn-is-not-what-you-think suggests that IAM restrictions are based on a specific Principal ARN.
This means my assumed identity does not match the required Principal ARN in the policy.
  - IAM Policy Analysis
<details> <summary>  Click to expand the IAM Policy</summary>
json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                },
                "ForAllValues:StringLike": {
                    "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
                }
            }
        }
    ]
}
</details>
* Key Findings from the IAM Policy
- First Statement:

Allows anyone (Principal: *) to retrieve (s3:GetObject) objects from the bucket.
This explains why I was able to download flag-as-admin.txt without authentication.
- Second Statement:

Restricts s3:ListBucket permissions to users with an IAM Principal ARN of arn:aws:iam::133713371337:user/admin.
Since my IAM identity does not match this ARN, my request was denied.
* Final Takeaways
* Success:

I was able to retrieve flag-as-admin.txt because s3:GetObject permissions are open to all users.
 Failure:

I was denied access when listing objects because my IAM role did not match the required Principal ARN.
- Big Lesson Learned:

Even when objects are publicly accessible, the IAM Principal ARN restriction prevents certain actions (like listing files).
This challenge highlights the importance of IAM conditions in security policies and how they affect AWS access control.
- Conclusion: Mastering IAM Policies
This challenge reinforced my understanding of:
* S3 bucket policies and conditions
* Public vs. restricted access control
* How IAM Principal ARNs affect permissions
