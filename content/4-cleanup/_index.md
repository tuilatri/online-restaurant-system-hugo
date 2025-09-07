---
title : "Cleaning Up Resources"
date : "2025-09-04" 
weight : 4
chapter : false
pre : " <b> 4. </b> "
---

{{% notice warning %}}
**EXTREMELY IMPORTANT:** This is a mandatory step after you complete the workshop. Failing to clean up resources will cause your AWS account to continue incurring costs, especially for services like NAT Gateway, Application Load Balancer, and EC2.
{{% /notice %}}

To ensure nothing is missed, we must delete the resources in the reverse order of their creation.

---

🔒 **Execution Steps**

#### **1. Delete CloudFront Distributions**
You need to delete both distributions created for the Frontend (S3) and Backend (ALB).

*   Navigate to the **CloudFront** service.
*   Select a distribution, click **Disable**, and confirm.
*   After the distribution is disabled, select it again and click **Delete**.
*   Repeat the process for the remaining distribution.

#### **2. Delete the Auto Scaling Group**
This action will also automatically terminate the EC2 instances managed by the ASG.

*   Navigate to the **EC2** service -> **Auto Scaling Groups**.
*   Select `project-backend-asg` and click **Delete**.
*   Type `delete` to confirm and complete the action.

#### **3. Delete the Application Load Balancer**
*   Navigate to **EC2** -> **Load Balancers**.
*   Select `project-backend-alb`, go to **Actions**, and choose **Delete load balancer**.
*   Type `confirm` to confirm.

#### **4. Delete the Target Group**
*   Navigate to **EC2** -> **Target Groups**.
*   Select `project-backend-target-group`, go to **Actions**, and choose **Delete**.

#### **5. Terminate Remaining EC2 Instances**
Instances not managed by the ASG must be terminated manually.

*   Navigate to **EC2** -> **Instances**.
*   Select the `project-bastion-host-ec2` and `project-backend-ec2` instances (if the latter wasn't already terminated by the ASG).
*   Select **Instance state** -> **Terminate instance**.

#### **6. Delete the Launch Template**
*   Navigate to **EC2** -> **Launch Templates**.
*   Select `project-backend-lauch-template`, go to **Actions**, and choose **Delete template**.

#### **7. Delete the AMI and Related Snapshot**
*   **Step 1: Deregister the AMI**
    *   Navigate to **EC2** -> **AMIs**.
    *   Select `project-backend-ec2-ami`, go to **Actions**, and choose **Deregister AMI**.
*   **Step 2: Delete the Snapshot**
    *   Navigate to **EC2** -> **Snapshots**.
    *   Find the Snapshot created by the AMI (the description usually contains the AMI ID), select it, and go to **Actions** -> **Delete snapshot**.

#### **8. Delete the S3 Bucket**
You must first delete all objects inside the bucket.

*   Navigate to the **S3** service.
*   Go into the `project-frontend-030925` bucket.
*   Select all files and click **Delete**.
*   Once the bucket is empty, go back, select the bucket, and click **Delete**.

#### **9. Delete the VPC**
This is the final step, which will delete the VPC and its associated resources like Subnets, Route Tables, Internet Gateway, and most importantly, the **NAT Gateway**.

*   Navigate to the **VPC** service.
*   Select **Your VPCs**, and choose `project-vpc`.
*   Click **Actions** -> **Delete VPC**.
*   A window will appear listing the resources to be deleted. Type `delete` and confirm.

#### **10. Delete the Key Pair**
*   Navigate to **EC2** -> **Key Pairs**.
*   Select `project-keypair`, go to **Actions**, and choose **Delete**.