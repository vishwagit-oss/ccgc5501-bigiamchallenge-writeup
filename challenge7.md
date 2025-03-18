
# CloudFox AWS CTF - "It's A Secret" Challenge Write-up

---

## Challenge Statement
The goal of this challenge is to retrieve a flag stored as an AWS **SSM Parameter**.

We start with an IAM user that has the following policies:

- **SecurityAudit** (AWS Managed)
- **CloudFox** (Customer Managed)
- **its-a-secret** (Customer Managed)

Using **CloudFox**, we need to discover the secret and extract the flag.

---

## IAM Policy

### IAM Policy: `its-a-secret`
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ssm:GetParameter",
                "ssm:GetParameters",
                "ssm:DescribeParameters"
            ],
            "Resource": "arn:aws:ssm:ca-central-1:585008067178:parameter/cloudfoxable/flag/its-a-secret"
        }
    ]
}
```

### Analysis  

## What do I have access to?
- I can retrieve **SSM Parameters** (`ssm:GetParameter`, `ssm:GetParameters`).
- I can list parameters using `ssm:DescribeParameters`.
- My access is **restricted** to only one parameter: **`/cloudfoxable/flag/its-a-secret`**.

## What don’t I have access to?
- I cannot access **other parameters** or **Secrets Manager** secrets.
- I cannot **modify** or **delete** parameters.
- I don’t have full access to AWS **SSM Parameter Store**.

## What’s interesting?
- The policy grants **just enough** access to retrieve the flag.
- This is an example of **least privilege** IAM policy design.

---

## Solution  

### Step 1: Confirm AWS Identity  
Verify that the correct profile is in use:  

```powershell
aws --profile ctf-starting-user sts get-caller-identity
```

**Output:**
```json
{
    "UserId": "AIDAYQNJSWJVMPCMN3YHS",
    "Account": "585008067178",
    "Arn": "arn:aws:iam::585008067178:user/cloudfoxable-admin"
}
```

---

### Step 2: Run CloudFox to Enumerate Secrets  
Use **CloudFox** to list accessible secrets:  

```powershell
cloudfox aws -p ctf-starting-user secrets -v2
```

**Output (Relevant Part):**
```
│ SSM            │ ca-central-1 │ /cloudfoxable/flag/its-a-secret       │                                  │
```

---

### Step 3: Retrieve the Secret  
Use **AWS CLI** to fetch the flag:  

```powershell
aws --profile ctf-starting-user ssm get-parameter --name "/cloudfoxable/flag/its-a-secret" --with-decryption
```

**Output:**
```json
{
    "Parameter": {
        "Name": "/cloudfoxable/flag/its-a-secret",
        "Type": "SecureString",
        "Value": "FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}",
        "Version": 1,
        "LastModifiedDate": "2025-02-19T21:45:15.624000-05:00",
        "ARN": "arn:aws:ssm:ca-central-1:585008067178:parameter/cloudfoxable/flag/its-a-secret",
        "DataType": "text"
    }
}
```

---

### Step 4: Submit the Flag  
Copy and submit the flag:  

```
FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}
```

---

## Reflection  

## What was my approach?
- First, I confirmed my **AWS identity**.
- Then, I used **CloudFox** to scan for accessible **secrets**.
- Finally, I used **AWS CLI** to retrieve the **flag** from **SSM Parameter Store**.

## What was the biggest challenge?
- Initially, I tried using the **cloudfoxable** profile, but it didn’t exist.
- The correct profile was **ctf-starting-user**.

## How did I overcome the challenge?
- I ran `aws configure list-profiles` to find available profiles.
- Once I switched to **ctf-starting-user**, CloudFox worked correctly.

## What led to the breakthrough?
- Checking the **IAM policy** helped me realize that I had access to only one parameter.
- Running **CloudFox secrets** quickly identified the **SSM parameter name**.

## On the blue team side, how can this learning be used for defense?
- **Use IAM conditions** to limit access to specific roles or IP addresses.
- **Enable logging** to monitor access to secrets.
- **Rotate secrets** regularly to prevent misuse.
- **Limit permissions** to prevent overexposure of secrets.

---

## Cleanup Tasks  
No cleanup is required.

---

### **Challenge Completed! **
