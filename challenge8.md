# **CloudFox AWS CTF - "It's A Secret" Challenge Write-up**

## **Challenge Details**
For this CTF, the starting `ctf-starting-user` has the following policies:

- **SecurityAudit** (AWS Managed)
- **CloudFox** (Customer Managed)
- **its-a-secret** (Customer Managed)

These policies allow the user to:
- Run **CloudFox** for reconnaissance.
- Retrieve the secret flag stored in AWS **SSM Parameter Store**.

If the setup was followed correctly, the profile `cloudfoxable` should be tied to the `ctf-starting-user`.

---

## **Step 1: Confirm AWS Identity**
Verify the AWS profile and check the identity:

```powershell
aws --profile ctf-starting-user sts get-caller-identity
Output:

json
Copy
Edit
{
    "UserId": "AIDAYQNJSWJVMPCMN3YHS",
    "Account": "585008067178",
    "Arn": "arn:aws:iam::585008067178:user/cloudfoxable-admin"
}
Step 2: Run CloudFox to Discover Secrets
Use CloudFox to enumerate secrets:

powershell
Copy
Edit
cloudfox aws -p ctf-starting-user secrets -v2
Output (Relevant Part):

bash
Copy
Edit
│ SSM            │ ca-central-1 │ /cloudfoxable/flag/its-a-secret       │                                  │
CloudFox also writes output files:

Secrets List:
C:\Users\vishw\.cloudfox\cloudfox-output\aws\ctf-starting-user-585008067178\table/secrets.txt

Loot File:
C:\Users\vishw\.cloudfox\cloudfox-output\aws\ctf-starting-user-585008067178\loot\pull-secrets-commands.txt

To view the loot file:

powershell
Copy
Edit
cat "C:\Users\vishw\.cloudfox\cloudfox-output\aws\ctf-starting-user-585008067178\loot\pull-secrets-commands.txt"
Step 3: Retrieve the Flag
Use the loot file’s command to retrieve the secret:

powershell
Copy
Edit
aws --profile ctf-starting-user ssm get-parameter --name "/cloudfoxable/flag/its-a-secret" --with-decryption
Output:

json
Copy
Edit
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
Step 4: Submit the Flag
Copy the flag:

cpp
Copy
Edit
FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}


Bonus: Verify Why This User Can Access the Secret
Run the following command to analyze permissions:

powershell
Copy
Edit
cloudfox aws -p cloudfoxable permissions --principal ctf-starting-user -v2
