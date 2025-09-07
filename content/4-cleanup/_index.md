---
title : "Clean up Resources"
date : "2025-09-06"
weight : 4
chapter : false
pre : "<b> 4. </b>"
---

To avoid incurring unwanted costs, it's important to delete all the AWS resources that were created during this workshop.

{{% notice warning %}}
**The deletion order is very important!**
Please follow the steps below sequentially. Deleting resources in the wrong order (e.g., deleting the VPC before the ALB) will cause errors because the resources are still dependent on each other.
{{% /notice %}}

#### **1. Delete the Auto Scaling Group (`ors-asg`)**

This is the **most important** first step. Otherwise, the ASG will automatically recreate the EC2 instances after you delete them.

*   Navigate to the **EC2** service -> **Auto Scaling Groups**.
*   Select `ors-asg`.
*   Click **Delete**.
*   Type `delete` in the confirmation box and click **Delete**. This action will also automatically terminate the EC2 instances managed by the ASG.

#### **2. Delete the Application Load Balancer (`ors-alb`)**

*   In the **EC2 Dashboard**, go to **Load Balancers**.
*   Select `ors-alb`.
*   Click **Actions** -> **Delete load balancer**.
*   Type `confirm` in the confirmation box and click **Delete**.

#### **3. Delete the Target Group (`ors-target-group`)**

*   In the **EC2 Dashboard**, go to **Target Groups**.
*   Select `ors-target-group`.
*   Click **Actions** -> **Delete**.
*   Confirm the deletion.

#### **4. Delete the EC2 Instance and related resources**

*   **Manually Terminate EC2 Instance:**
    *   Go to **Instances**.
    *   Select `ors-ec2` (the instance you created initially).
    *   Click **Instance state** -> **Terminate instance**.
*   **Deregister AMI:**
    *   Go to **AMIs** (under the Images section).
    *   Select `ors-ami`.
    *   Click **Actions** -> **Deregister AMI**.
*   **Delete Launch Template:**
    *   Go to **Launch Templates**.
    *   Select `ors-launch-template`.
    *   Click **Actions** -> **Delete template**.
*   **Delete Key Pair:**
    *   Go to **Key Pairs** (under the Network & Security section).
    *   Select `ors-keypair`.
    *   Click **Actions** -> **Delete**.

#### **5. Delete the RDS Database (`ors-db`)**

*   Navigate to the **RDS** service -> **Databases**.
*   Select `ors-db`.
*   Click **Actions** -> **Delete**.
*   **Uncheck** the `Create final snapshot?` box.
*   **Check** the `I acknowledge...` box.
*   Type `delete me` in the confirmation box and click **Delete**.

{{% notice info %}}
The RDS database deletion process may take a few minutes.
{{% /notice %}}

#### **6. Delete CloudFront Distributions**

You need to repeat this process for **both** distributions you created (one for the backend and one for the frontend).

*   Navigate to the **CloudFront** service.
*   Select a distribution.
*   Click **Disable**. Wait a few minutes until the status updates (the Last modified column shows a value).
*   Once it is disabled, select the distribution again and click **Delete**.

#### **7. Delete the S3 Bucket (`ors-fe`)**

*   Navigate to the **S3** service.
*   Select the `ors-fe` bucket.
*   Click the **Empty** button.
*   Type `permanently delete` in the confirmation box and click **Empty**.
*   After the bucket is empty, go back to the bucket list, select `ors-fe` again, and click **Delete**.
*   Type the bucket name in the confirmation box and click **Delete bucket**.

#### **8. Delete the VPC (`ors-vpc`)**

This is the final step, which deletes the entire network environment.

*   Navigate to the **VPC** service -> **Your VPCs**.
*   Select `ors-vpc`.
*   Click **Actions** -> **Delete VPC**.
*   A window will appear, listing all related resources that will be deleted along with it (Subnets, Route Tables, Security Groups, etc.).
*   Type `delete vpc` in the confirmation box and click **Delete**.