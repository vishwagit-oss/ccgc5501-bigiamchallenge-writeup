# Bastion Challenge Write-up

## CloudFoxable Setup:

1. Edit `cloudfoxable/aws/terraform.tfvars`.
2. Switch the challenge flag to true: `bastion_enabled = true`.
3. Run `terraform apply` to deploy the setup.

## Challenge Details:

This challenge sets up your Bastion host that you can use for all other challenges in the "Assumed Breach: Application Compromise/Network Access" category.

### Steps I took:

1. The easiest way to access an EC2 instance is by using SSM.
2. Once the challenge is deployed, I ran `cloudfox aws -p cloudfoxable instances -v2` to find the instance ID.
3. I installed the Session Manager plugin to connect to the EC2 instance.
4. I used the loot file from the `instances` command to find the command needed to connect via SSM.
5. From there, I used CloudFox to figure out what IAM permissions I had access to and found the flag.

## Cleanup Tasks:

1. Edit `cloudfoxable/aws/terraform.tfvars` and switch the challenge flag to false: `bastion_enabled = false`.
2. Run `terraform apply` to finalize the cleanup.

## Unlock Hint for 0 points:

To figure out what IAM permissions you have, use the following commands:
- `cloudfox aws -p cloudfoxable permissions`
- `cloudfox aws -p cloudfoxable permissions --principal [NameOfInstanceRole]`

## Flag

After completing the challenge, I found the flag:
{FLAG:bastion::ifYouHaveAccessToAnEC2YouHaveAccessToItsIamPermissions}

---


