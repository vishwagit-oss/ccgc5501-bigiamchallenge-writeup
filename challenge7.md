# Cloudfoxable - Bastion 100 Write-Up

## Overview

The **Bastion 100** challenge is part of the "Assumed Breach: Application Compromise/Network Access" category. This challenge focuses on setting up and leveraging a Bastion host, which is a critical component for securely accessing internal network resources, such as EC2 instances in this case. The challenge involves using AWS services like EC2 and IAM, and specifically focuses on utilizing AWS Systems Manager (SSM) for accessing an EC2 instance.



## CloudFoxable Setup

To get started with the challenge, I had to configure the environment correctly. The setup process was straightforward:

1. First, I navigated to the `cloudfoxable/aws/terraform.tfvars` file and updated it to enable the Bastion host by setting the `bastion_enabled` flag to `true`. The configuration file looked something like this:

   ```hcl
   bastion_enabled = true
After this, I applied the Terraform changes using the command:


terraform apply
This command deployed the necessary infrastructure for the challenge, including the Bastion host that I would use to access internal resources.

Challenge Steps
1. Identify the EC2 Instance
Once the Bastion host was set up, I needed to find the EC2 instance that I would be working with. The easiest way to do this was to use the CloudFox tool. I ran the following command to list all the instances and their details:

cloudfox aws -p cloudfoxable instances -v2
This command displayed all the EC2 instances in the setup, and from the output, I could identify the instance I needed to target.

2. Install the Session Manager Plugin
To interact with the EC2 instance, I decided to use AWS Systems Manager (SSM), which allows you to securely access EC2 instances without needing to open SSH ports. For this, I first needed to install the AWS Session Manager plugin.

You can install the Session Manager plugin by following the instructions in the AWS documentation.

Once installed, the Session Manager plugin would allow me to connect to the EC2 instance using SSM commands.

3. Access the EC2 Instance Using SSM
With the plugin installed, I needed to find the correct command to connect to the EC2 instance via SSM. I checked the loot file that was outputted from the cloudfox aws -p cloudfoxable instances -v2 command. The loot file provided the command that I would need to run to connect via SSM.

For example, the command might look something like this:


aws ssm start-session --target <instance_id>
I ran this command and successfully connected to the EC2 instance using SSM.

4. Explore the Instance to Find IAM Permissions
Once I had access to the EC2 instance, I explored its environment to gather information about the IAM permissions it had. The goal was to understand what actions I could perform and see if I had access to anything that would help me find the flag.

To identify the permissions of the EC2 instance, I used the CloudFox tool again, with the following command:

cloudfox aws -p cloudfoxable permissions
This command helped me list the IAM permissions granted to the instance role. From there, I could explore further to see if there were any elevated permissions or accessible resources that might lead to the flag.

5. Finding the Flag
Through the exploration of the EC2 instance and understanding the permissions, I found the flag. The flag was related to the IAM permissions of the EC2 instance, as it granted access to its IAM permissions, which could potentially open the door to further resources.

The flag I found was:


{FLAG:bastion::ifYouHaveAccessToAnEC2YouHaveAccessToItsIamPermissions}
This flag confirmed the key lesson of the challenge — once you have access to an EC2 instance, you also have access to its IAM permissions, which could potentially open the door to further resources.

Cleanup Tasks
After completing the challenge and finding the flag, I performed the following cleanup tasks to disable the Bastion host and restore the environment to its original state:

I navigated back to the cloudfoxable/aws/terraform.tfvars file.

I set the bastion_enabled flag to false:

bastion_enabled = false
I ran the terraform apply command again to ensure the Bastion host was properly disabled:


terraform apply

If you need help understanding what IAM permissions are accessible, you can use the following CloudFox commands:

cloudfox aws -p cloudfoxable permissions
cloudfox aws -p cloudfoxable permissions --principal [NameOfInstanceRole]
These commands help reveal the permissions associated with the EC2 instance and can guide you toward the flag.

Conclusion
This challenge highlighted the importance of understanding IAM permissions and how a Bastion host can be used to access an EC2 instance. By leveraging AWS SSM, I was able to securely access the instance and explore its IAM permissions to find the flag.
