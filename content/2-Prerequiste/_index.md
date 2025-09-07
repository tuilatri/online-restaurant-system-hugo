---
title : "Preparation Steps"
date : "2025-09-04"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

This page provides an overview of the necessary steps to deploy the weather application project onto the AWS environment. Each section in the list below will lead you to a detailed instruction page.

{{% notice info %}}
Please follow the steps sequentially to ensure the infrastructure is set up correctly.
{{% /notice %}}

### Main Workshop Content

1.  **Configure the Network Environment (VPC)**
    *   [Create VPC](2.1-createvpc/)
    *   [Edit Public Subnets](2.2-modifysubnet/)

2.  **Set up Security Layers (Security Group)**
    *   [Create Security Groups for Components](2.3-createsg/)

3.  **Initialize Virtual Servers (EC2)**
    *   [Create EC2 Instances for Bastion Host and Backend](2.4-createec2/)

4.  **Prepare for Scaling (AMI & Launch Template)**
    *   [Create Amazon Machine Image (AMI)](2.5-createami/)
    *   [Create Launch Template for Auto Scaling](2.6-createlaunchtemplate/)

5.  **Set up Load Balancing (Load Balancer & Target Group)**
    *   [Create Target Group](2.7-createtargetgroup/)
    *   [Create Application Load Balancer (ALB)](2.8-createalb/)

6.  **Configure Auto Scaling (Auto Scaling Group)**
    *   [Create Auto Scaling Group](2.9-createasg/)

7.  **Store Frontend on S3**
    *   [Create S3 Bucket and Configure Static Website Hosting](2.11-creates3/)

8.  **Accelerate and Distribute Content (CloudFront)**
    *   [Create CloudFront for Backend (ALB)](2.10-createcloudfrontbe/)
    *   [Create CloudFront for Frontend (S3)](2.12-createcloudfrontfe/)