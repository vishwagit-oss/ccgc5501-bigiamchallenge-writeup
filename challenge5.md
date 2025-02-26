# First Flag - CloudFoxable CTF

## Overview

**Author:** Seth Art (@sethsec)  
**Default State:** Enabled  
**Estimated Cost:** No cost  
**Starting Point:** [CloudFoxable GitHub Repository](https://github.com/BishopFox/cloudfoxable)  

## Welcome!

CloudFoxable is Terraform-based AWS infrastructure designed for security testing. The repository is available at [BishopFox/CloudFoxable](https://github.com/BishopFox/cloudfoxable). This walkthrough details the process I followed to deploy CloudFoxable, find the first flag, and clean up resources.

---

## Setup Instructions

### Step 1: Prepare AWS Environment
1. Selected a new AWS playground account to avoid using any production resources.
2. Created a **non-root IAM user** with **administrative privileges**.
3. Generated an **access key** for this user.
4. Installed and configured the **AWS CLI**:
   ```sh
   aws configure
Verified my AWS CLI setup with:
sh
Copy
Edit
aws sts get-caller-identity
Step 2: Install Terraform
Downloaded and installed Terraform.
Added Terraform binary to my system path.
Step 3: Clone and Deploy CloudFoxable
Cloned the repository:
sh
Copy
Edit
git clone https://github.com/BishopFox/cloudfoxable
Navigated to the AWS directory:
sh
Copy
Edit
cd cloudfoxable/aws
Copied the example Terraform variables file:
sh
Copy
Edit
cp terraform.tfvars.example terraform.tfvars
Initialized Terraform:
sh
Copy
Edit
terraform init
Edited terraform.tfvars and set my AWS profile:
sh
Copy
Edit
aws_local_profile = "YOUR_PROFILE"
Optionally reviewed the Terraform execution plan:
sh
Copy
Edit
terraform plan
Applied the Terraform configuration:
sh
Copy
Edit
terraform apply
Step 4: Install Additional Tools
Installed necessary AWS security assessment tools:

CloudFox
Pmapper
Pacu
Finding the First Flag
After running terraform apply, I closely reviewed the Terraform output. The output contained instructions on setting up the ctf-starting-user with the AWS CLI. Following these instructions, I was able to locate the first flag.

Found a flag format in the Terraform output:

sh
Copy
Edit
FLAG{challengeName::CamelCaseText}
Extracted the flag from the Terraform output:

sh
Copy
Edit
FLAG{congrats_you_are_now_a_terraform_expert_happy_hunting}
Once identified, I submitted the entire flag string as required.

Clean Up
To remove all CloudFoxable challenges and resources, I ran the following command from the cloudfoxable/aws directory:

sh
Copy
Edit
terraform destroy
This ensured that all deployed resources were properly deleted.

###Conclusion
Successfully deployed CloudFoxable, found the first flag, and cleaned up the environment. This exercise provided hands-on experience with AWS security testing and Terraform-based infrastructure deployment.

