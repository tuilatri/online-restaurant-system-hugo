---
title : "Create Launch Template"
date : "2025-09-06"
weight : 7
chapter : false
pre : " <b> 2.7 </b> "
---

### Create a Launch Template

ℹ️ **Objective**

*   A **Launch Template** acts as a detailed "blueprint" for launching an EC2 instance. It stores all necessary configuration parameters such as the AMI, instance type, key pair, and security group.
*   Our goal is to create a Launch Template that uses the web server's **AMI** (`ors-ami`) created in the previous step. The **Auto Scaling Group** will rely on this blueprint to automatically launch new instances consistently and accurately.

---

🔒 **Steps to Follow**

#### **1. Access Launch Templates**

*   From the **EC2 Dashboard**, on the left menu, scroll down to the **Instances** section and select **Launch Templates**.
*   Click on **Create launch template**.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-dashboard.png" title="Start creating a Launch Template" >}}

#### **2. Configure Basic Information**

*   **Launch template name:** `ors-launch-template`
*   **Template version description:** `Launch template for Online Restaurant System servers`

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-basic-info.png" title="Enter basic information for the Launch Template" >}}

#### **3. Select AMI (Amazon Machine Image)**

*   In the **Application and OS Images (Amazon Machine Image)** section, select the **My AMIs** tab.
*   Select **Owned by me** and you will see the `ors-ami` AMI created in the previous step. Select it.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-select-ami.png" title="Select the created ors-ami AMI" >}}

#### **4. Select Instance Type and Key Pair**

*   **Instance type:** Select `t2.micro`.
*   **Key pair (login):** Select `ors-keypair` from the dropdown list.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-instance-type-keypair.png" title="Select Instance Type and Key Pair" >}}

#### **5. Configure Network Settings**

*   In the **Network settings** section, we do **not** need to select a Subnet, as the Auto Scaling Group will decide this automatically.
*   However, we do need to specify the **Security Group**.
*   **Security groups:** Choose **Select existing security group**.
*   From the **Common security groups** list, select `ors-sg`.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-network-settings.png" title="Configure the Security Group for the Launch Template" >}}

#### **6. Finalize and Review the Launch Template**

*   Review all the configured information in the **Summary** panel on the right.
*   Scroll down and click **Create launch template**.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-create.png" title="Complete the Launch Template creation" >}}

*   After successful creation, click **View launch templates** to review the "blueprint" you just created in the list.

{{< figure src="/images/2.prerequisite/2.7-createlaunchtemplate/lt-view-list.png" title="View the Launch Template in the list" >}}

{{% notice success %}}
You now have a complete blueprint. Whenever the system needs a new web server, it can simply use this template to create a perfect copy.
{{% /notice %}}