
#  CloudFoxable Setup - "It's Another Secret"

## Challenge Statement

The challenge I am working on involves assuming the role **Ertz** to find and access the **its-another-secret** flag. The objective is to simulate gaining access to the **Ertz** role, checking its permissions, and locating the secret.

## IAM Policy

For this challenge, the IAM policy associated with the **Ertz** role was not explicitly provided. However, based on the challenge, we assume the following:

- **Role:** Ertz
- **Permissions:** The permissions granted to the **Ertz** role allow assuming the role and accessing secrets or other cloud resources.
  
### IAM Policy Analysis

- **What do I have access to?**
  - I have access to assume the **Ertz** role via AWS CLI using the `sts assume-role` command.
  - I have access to various AWS services based on the permissions of the **Ertz** role. This likely includes access to secrets, IAM, and other resources that might store the flag.
  
- **What don't I have access to?**
  - I do not have any specific access to resources outside the scope of the **Ertz** role. If the role does not have broad permissions, I might not be able to access other AWS services without further escalation.
  
- **What do I find interesting?**
  - The challenge emphasizes the importance of **role assumption**, which is critical in cloud penetration testing.
  - The role assumption process allowed me to explore permissions and find the secret flag hidden in the environment.

## Solution

### Step 1: Assume the Ertz Role

To assume the **Ertz** role, I used two possible methods. I chose the easier method of editing the AWS configuration to automatically assume the role.

1. Edited the `~/.aws/config` file to add the following configuration:

    ```ini
    [profile ertz]
    region = us-west-2
    role_arn = arn:aws:iam::ACCOUNT_ID:role/Ertz
    source_profile = cloudfoxable
    ```

2. Verified the role assumption with the command:

    ```bash
    aws --profile ertz sts get-caller-identity
    ```

    This confirmed that I was assuming the **Ertz** role with the expected session ID and ARN.

### Step 2: Check Permissions of the Ertz Role

I checked the permissions of the **Ertz** role to understand what actions I could take with it. I used the AWS CLI to list the policies attached to the role.

### Step 3: Find the "its-another-secret" Secret

I explored the AWS environment for any secrets related to the **its-another-secret** flag. After searching through the accessible resources, I used AWS Secrets Manager to list and retrieve secrets.

    ```bash
    aws --profile ertz secretsmanager list-secrets
    ```

Upon finding the relevant secret, I retrieved the flag:

```
FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}
```

### Conclusion

By assuming the **Ertz** role and exploring its permissions, I successfully located the flag. This challenge helped me practice role assumption, a critical skill in cloud penetration testing.

## Reflection

### What was your approach?

My approach was to first assume the **Ertz** role using the provided instructions, then check the role's permissions to understand what resources I could access. Once I had the role, I searched for the secret that would lead me to the flag.

### What was the biggest challenge?

The biggest challenge was ensuring that I was correctly assuming the **Ertz** role and navigating the permissions effectively to access the correct resources. Role assumption can sometimes be tricky, especially if permissions are restricted.

### How did you overcome the challenges?

I overcame these challenges by following the instructions closely, ensuring I edited the AWS CLI configuration properly to assume the role without issues. Additionally, I made sure to validate my role assumption with the `sts get-caller-identity` command before proceeding.

### What led to the breakthrough?

The breakthrough came when I successfully assumed the **Ertz** role and began exploring its permissions. Once I had access to the role, it was just a matter of searching for the secret in the relevant resources.

### On the blue side, how can the learning be used to properly defend the important assets?

The lessons learned from this challenge can help in defending cloud assets by ensuring that roles are properly configured with the least privilege principle in mind. Regular audits of roles, permissions, and secret management systems like AWS Secrets Manager are crucial for maintaining a secure cloud environment. Additionally, strong role assumption controls and monitoring can help detect and mitigate unauthorized role assumptions.
