# CloudFoxable Setup - "It's Another Secret"

## Challenge Overview

In this challenge, I gained access to the role **Ertz**. The main objective was to assume the role and find and access the **its-another-secret** flag. This challenge simulates a scenario where I've just gained access to the role **Ertz** and need to perform a cloud penetration test by assuming roles and exploring permissions.

## Steps Taken

### Step 1: Assume the Role "Ertz"

The challenge provides two ways to assume the role **Ertz**:

- **The Hard Way:** Use AWS CLI to manually assume the role:
  
  ```bash
  aws --profile cloudfoxable sts assume-role --role-arn arn:aws:iam::ACCOUNT_ID:role/Ertz --role-session-name Ertz
  ```

  Then, use the output to set up new profiles or environment variables.

- **The Easy Way:** Create a new AWS CLI profile that handles the role assumption automatically. I edited the `~/.aws/config` file and added the following configuration:

  ```ini
  [profile ertz]
  region = us-west-2
  role_arn = arn:aws:iam::ACCOUNT_ID:role/Ertz
  source_profile = cloudfoxable
  ```

  With this configuration, I can use the **ertz** profile directly in AWS CLI commands.

### Step 2: Verify Role Assumption

To confirm that the role assumption was successful, I ran the following command:

```bash
aws --profile ertz sts get-caller-identity
```

The output showed that I was correctly assuming the **Ertz** role:

```json
{
    "UserId": "AROAQXHJKLZKFYSRACOES:botocore-session-1684201365",
    "Account": "ACCOUNT_ID",
    "Arn": "arn:aws:sts::ACCOUNT_ID:assumed-role/Ertz/botocore-session-1684201365"
}
```

### Step 3: Check Permissions for the Ertz Role

Once I had assumed the **Ertz** role, the next step was to check the permissions granted to the role. This is essential in any cloud penetration test to identify what the assumed user can and cannot do. I inspected the permissions associated with the **Ertz** role.

### Step 4: Find the "its-another-secret" Secret

I navigated through the environment, looking for a secret or flag. After thoroughly searching the accessible resources, I discovered the flag.

### Step 5: Flag Discovery

The flag for this challenge was:

```
FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}
```

## Conclusion

This challenge was a great exercise in assuming AWS roles and exploring permissions. By using a simple profile configuration, I was able to seamlessly assume the **Ertz** role and locate the flag. The challenge emphasized the importance of role assumption in cloud penetration tests and how it can lead to uncovering valuable information.

---
