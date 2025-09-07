---
title : "Create S3 Bucket for Frontend"
date : "2025-09-06"
weight : 11
chapter : false
pre : " <b> 2.11 </b> "
---

### Create an S3 Bucket to Store the Frontend

ℹ️ **Objective**

*   **Amazon S3 (Simple Storage Service)** is an object storage service that offers scalability, high durability, and low cost.
*   We will use S3 to store all the static files of our frontend application (HTML, CSS, JavaScript, images).
*   Afterward, we will configure this bucket to function as a **Static Website Hosting** server.

---

🔒 **Steps to Follow**

#### **1. Start Creating the S3 Bucket**

*   In the **AWS Management Console**, search for and select the **S3** service.
*   Click on **Create bucket**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-dashboard.png" title="Start creating an S3 Bucket" >}}

#### **2. Configure the Bucket**

*   **Bucket name:** `ors-fe`
    {{% notice warning %}}
    S3 bucket names are globally unique. If the name `ors-fe` is already in use, you will need to add characters or numbers to make it unique (e.g., `ors-fe-haminhtri-0309`).
    {{% /notice %}}

*   **AWS Region:** Choose the region you are working in.

*   **Block Public Access settings for this bucket:**
    *   **Check** the box for **Block all public access**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-unblock-public-access.png" title="Block public access for the bucket" >}}

*   **Bucket Versioning:** Select `Enable`.
*   Scroll down and click **Create bucket**.

#### **3. Upload Frontend Files to the Bucket**

*   After the bucket is created successfully, go into the `ors-fe` bucket.
*   Click **Upload** and upload all the files and folders of your frontend project.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-upload-files.png" title="Upload Frontend files to S3" >}}

#### **4. Configure Static Website Hosting**

*   In the `ors-fe` bucket, switch to the **Properties** tab.
*   Scroll to the bottom to the **Static website hosting** section and click **Edit**.
*   Select **Enable**.
*   **Index document:** `index.html`
*   Click **Save changes**.

{{< figure src="/images/2.prerequisite/2.11-creates3/s3-static-hosting-01.png" title="Enable and configure Static Website Hosting" >}}