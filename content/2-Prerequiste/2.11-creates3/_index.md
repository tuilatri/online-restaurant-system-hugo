---
title : "Create S3 Bucket and Configure Static Website Hosting"
date : "2025-09-04"
weight : 11
chapter : false
pre : " <b> 2.11 </b> "
---

### Create S3 Bucket to Store the Frontend

ℹ️ **Objective**

*   **Amazon S3 (Simple Storage Service)** is a highly scalable, durable, and low-cost object storage service.
*   We will use S3 to store all the static files of the frontend application (HTML, CSS, JavaScript, images).
*   Afterward, we will configure this bucket to act as a static web server (Static Website Hosting), allowing direct access to the website via an S3 URL.

---

🔒 **Execution Steps**

#### **1. Start Creating the S3 Bucket**

*   In the **AWS Management Console**, search for and select the **S3** service.
*   Click on **Create bucket**.

{{< figure src="/images/2.prerequisite/2.11-creates3/create-s3-button.png" title="Starting to create an S3 Bucket" >}}

#### **2. Configure Basic Information**

*   **Bucket name:** `project-frontend-030925`
    {{% notice warning %}}
    S3 bucket names are globally unique. If this name already exists, you need to add characters or numbers to make it unique (e.g., `project-frontend-030925-yourname`).
    {{% /notice %}}
*   **AWS Region:** Select the Region you are working in.

*   **Object Ownership:** Select `ACLs enabled`.
    {{% notice info %}}
    We select this option to be able to grant public access to individual files using Access Control Lists (ACL), a necessary method for the next configuration step.
    {{% /notice %}}

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-basic-config-acl.png" title="Configuring name, Region, and enabling ACLs" >}}

#### **3. Configure Public Access Settings**

*   Check the box for **Block all public access**.

{{% notice warning %}}
This step allows us to make objects in the bucket publicly accessible. In the following steps, CloudFront will be configured to protect this bucket more securely.
{{% /notice %}}

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-enable-block-public-access.png" title="Enabling Block Public Access" >}}

*   **Bucket Versioning:** Select `Enable`.
*   Scroll down and click **Create bucket**.

#### **4. Upload Frontend Files to the Bucket**

*   After successfully creating the bucket, go into the `project-frontend-030925` bucket.
*   Click **Upload** and upload all files and folders of your frontend project.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-upload-files.png" title="Uploading Frontend files to S3" >}}

#### **5. Configure Static Website Hosting**

*   In the bucket, switch to the **Properties** tab.
*   Scroll down to the bottom to the **Static website hosting** section and click **Edit**.
*   Select **Enable**.
*   **Index document:** `index.html`
*   Click **Save changes**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-static-hosting.png" title="Enabling and configuring Static Website Hosting" >}}