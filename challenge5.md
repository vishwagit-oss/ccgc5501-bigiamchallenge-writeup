Reflection on Setting Up a Personal AWS Environment
As part of this exercise, I deployed and configured my own AWS environment to host CloudFoxable, a Capture the Flag (CTF) challenge designed to explore AWS security concepts. This reflection documents the steps taken, security measures implemented, and key lessons learned.

Step-by-Step Reflection
Step 1: Creating a Dedicated AWS Account
What I Did:
I created a separate AWS account specifically for this CTF challenge instead of using my personal or existing AWS account.

Why I Did It:
Isolation of resources – Prevents interference with other AWS projects.
Minimized security risks – If any misconfigurations occur, they won’t affect production or personal workloads.
Cost control – Allows me to track and delete all CTF-related resources when finished.
Key Reflection:
Using a dedicated AWS account is a best practice for security testing. It ensures that any misconfigurations, privilege escalations, or security flaws discovered in the exercise do not impact sensitive or critical environments.

Step 2: Creating an Admin User Instead of Using the Root User
What I Did:
After setting up the AWS account, I created an IAM user with administrative privileges instead of using the root user.

Why I Did It:
Security best practice – The root user has unrestricted access and should not be used for daily operations.
Access control – IAM users can be assigned specific roles and permissions.
Auditability – AWS logs IAM user activity in CloudTrail, allowing for better tracking of actions.
Key Reflection:
Even though I had access to the root user, using an admin IAM user instead was a critical security step. It enforces better access control, reduces exposure to risks, and ensures actions are logged for auditing purposes.

Step 3: Configuring the AWS CLI Securely
What I Did:
I installed the AWS CLI and configured it using the newly created admin IAM user.

Commands used:

sh
Copy
Edit
aws configure
aws sts get-caller-identity
Why I Did It:
Automated access management – Using the CLI allows me to interact with AWS services more efficiently.
Verifying authentication – Running aws sts get-caller-identity confirmed that I was using the correct IAM user.
Avoiding manual errors – The CLI ensures consistency in executing AWS commands.
Key Reflection:
Setting up the AWS CLI correctly was essential for managing my environment securely and efficiently. Manually interacting with AWS via the web console can lead to misconfigurations, whereas CLI automation ensures consistent and repeatable actions.

Step 4: Deploying CloudFoxable with Terraform
What I Did:
I used Terraform to deploy CloudFoxable’s AWS infrastructure. The steps included:

sh
Copy
Edit
git clone https://github.com/BishopFox/cloudfoxable
cd cloudfoxable/aws
cp terraform.tfvars.example terraform.tfvars
terraform init
terraform apply
Why I Did It:
Infrastructure as Code (IaC) best practice – Terraform enables a structured, repeatable, and auditable deployment.
Minimizing manual configuration errors – Automates resource creation, reducing human mistakes.
Easy cleanup – terraform destroy removes all resources when finished.
Key Reflection:
Terraform made deploying AWS resources significantly easier, faster, and more secure compared to manually creating resources via the console. Automating infrastructure setup reduces misconfigurations and allows for better control over the environment.

Step 5: Configuring IAM Policies and Least Privilege Access
What I Did:
Assigned only the necessary permissions to IAM users and roles instead of using overly permissive policies.
Implemented least privilege access to ensure that users only had the permissions they needed.
Why I Did It:
Security best practice – Overly permissive IAM policies are a common security risk.
Prevention of privilege escalation attacks – Limiting access prevents attackers from exploiting misconfigured permissions.
Better compliance with AWS security standards – AWS recommends applying the principle of least privilege.
Key Reflection:
IAM security is one of the most critical aspects of AWS security. If IAM roles and permissions are not properly configured, it can lead to serious security vulnerabilities, including privilege escalation and data exposure.

Step 6: Installing AWS Security Tools
What I Did:
After deploying CloudFoxable, I installed additional AWS security assessment tools to analyze misconfigurations and potential attack paths. These included:

CloudFox – Helps visualize IAM permissions and privilege escalation opportunities.
Pmapper – Analyzes AWS IAM policies to find weak links.
Pacu – An AWS exploitation framework for security testing.
Why I Did It:
Understanding AWS privilege escalation risks – These tools simulate real-world attack scenarios.
Identifying misconfigurations – Helps analyze IAM relationships and permissions.
Hands-on security learning – Gained practical experience in AWS security testing.
Key Reflection:
These security tools provided valuable insights into AWS security weaknesses. By using them, I gained a deeper understanding of how attackers think and operate within AWS environments.

Step 7: Finding the Flag and Understanding the CTF Structure
What I Did:
I analyzed the Terraform output after deployment to locate the first challenge flag.

Flag format:

cpp
Copy
Edit
FLAG{challengeName::CamelCaseText}
Example:

Copy
Edit
FLAG{congrats_you_are_now_a_terraform_expert_happy_hunting}
Why I Did It:
Verifying a successful deployment – The flag confirmed that CloudFoxable was set up correctly.
Understanding AWS privilege escalation paths – The CTF challenges walk through real-world AWS security scenarios.
Key Reflection:
The CloudFoxable CTF structure is designed to simulate realistic AWS security risks. By participating in these challenges, I learned how to identify misconfigurations and privilege escalation techniques in AWS.

Step 8: Cleaning Up the Environment
What I Did:
After completing the challenge, I destroyed all AWS resources to avoid unnecessary costs.

Command used:

sh
Copy
Edit
terraform destroy
Why I Did It:
Avoid ongoing AWS charges – Even free-tier resources can incur costs if left running.
Remove security risks – Ensures no leftover misconfigurations or exposed credentials.
Best practice for temporary environments – CloudFoxable is meant for short-term use, not long-term deployments.
Key Reflection:
Cleaning up cloud environments is just as important as setting them up. Leaving unused AWS resources running can lead to unexpected costs and security risks.

Final Takeaways and Lessons Learned
IAM Security is Critical

Misconfigured IAM permissions are one of the biggest risks in AWS.
Applying least privilege access helps prevent privilege escalation.
The Root User Should Never Be Used for Regular Tasks

The admin IAM user provides better security, access control, and logging.
AWS security best practices strongly discourage using the root account.
Infrastructure as Code (IaC) Enhances Security and Efficiency

Terraform simplified the AWS resource deployment process while ensuring security and consistency.
Security Tools are Essential for AWS Assessments

Tools like CloudFox, Pmapper, and Pacu help identify privilege escalation paths and misconfigurations.
