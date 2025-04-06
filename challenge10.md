## Challenge Statement
In some challenges, you might not see an IAM role or an IP address as the starting point, but rather, an interesting ARN or something like that.

Sometimes during cloud penetration tests, we first find something interesting and then work backwards to see who has access to it. Is it just the Administrators? That’s not really a big deal. Is it all developers, or all users, or anyone in the world? That might be a *really* big deal!

In this challenge, we are tasked with finding out who has access to the **DomainAdministrator-Credentials** in Secrets Manager and then using the cloudfoxable profile to access that role and retrieve the secret.

---

## IAM Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": "arn:aws:secretsmanager:us-west-2:ACCOUNT_ID:secret:DomainAdministrator-Credentials-XXXXXX"
    }
  ]
}
```

### IAM Policy Analysis

- **What do I have access to?**
  - I have access to call `secretsmanager:GetSecretValue` on a specific secret: `DomainAdministrator-Credentials`.
- **What don’t I have access to?**
  - I don’t have permissions to list all secrets or manage IAM users, roles, or trust relationships directly.
- **What do I find interesting?**
  - The policy is very tightly scoped to one secret — a good practice.
  - However, access to this powerful credential could still be sensitive depending on who has this permission.

---

## Solution

### Step 1: Identify Who Has Access to the Secret

```bash
cloudfox aws -p cloudfoxable permissions -v2 | grep -i secret
```

- Found that the role `Alexander-Arnold` has access to the target secret via `secretsmanager:GetSecretValue`.

### Step 2: Check Trust Relationships

```bash
cloudfox -p cloudfoxable role-trusts -v2
```

- Discovered that `Alexander-Arnold` role trusts `ctf-starting-user`, the identity used by the `cloudfoxable` profile.
- Even though `ctf-starting-user` lacks `sts:AssumeRole` explicitly, trust policy allows it implicitly since it is in the same account.

### Step 3: Assume the Role

Created a new AWS CLI profile in `~/.aws/config`:

```ini
[profile Alexander-Arnold]
region = us-west-2
role_arn = arn:aws:iam::ACCOUNT_ID:role/Alexander-Arnold
source_profile = cloudfoxable
```

### Step 4: Retrieve the Secret

```bash
aws secretsmanager get-secret-value   --secret-id DomainAdministrator-Credentials   --profile Alexander-Arnold   --region us-west-2
```

- Secret retrieved successfully.

### Final Flag

```
FLAG{backwards::IfYouFindSomethingInterstingFindWhoHasAccessToIt}
```

---

## Reflection

### What was your approach?
- Use CloudFox enumeration tools (`permissions` and `role-trusts`) to trace access to the secret.

### What was the biggest challenge?
- Understanding the behavior of AWS role trust relationships — specifically how roles can be assumed even without explicit `sts:AssumeRole` permissions.

### How did you overcome the challenges?
- Revisited the trust policy documentation and relied on CloudFox’s output to make the logical connection.

### What led to the breakthrough?
- Finding the `Alexander-Arnold` role and realizing it explicitly trusted `ctf-starting-user` — despite missing `sts:AssumeRole` permissions in the original identity’s policy.

### Blue Team Takeaway
- Always validate both **permissions policies** and **trust policies**.
- Sensitive roles should not be assumable solely based on trust policy unless strictly required.
- Use least privilege and audit role assumption activity regularly.
