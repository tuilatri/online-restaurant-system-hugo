---
title : "Prerequisites and Infrastructure Setup"
date : "2025-09-06"
weight : 2
chapter : false
pre : " <b> 2. </b> "
---

This page provides an overview of the necessary steps to set up the complete infrastructure for the restaurant project on AWS. Each section in the list below will link to a detailed guide.

{{% notice info %}}
Please follow the steps below sequentially. Setting them up in the wrong order can lead to configuration errors and prevent components from communicating with each other.
{{% /notice %}}

### Main Content of This Chapter

1.  **Configure the Network Environment (VPC)**
    *   [Create VPC and Subnets](2.1-createvpc/)
    *   [Modify Public Subnet](2.2-modifysubnet/)

2.  **Set Up Security Layers (Security Group)**
    *   [Create Security Groups for Web Server and Database](2.3-createsg/)

3.  **Initialize Server and Database**
    *   [Create an initial EC2 Instance for configuration](2.4-createec2/)
    *   [Create an RDS MySQL Database](2.5-createrds/)
    *(Note: The RDS creation step has been added here to ensure a seamless workshop flow)*

4.  **Prepare for Automatic Scaling (Automation)**
    *   [Create an Amazon Machine Image (AMI) from the configured EC2](2.6-createami/)
    *   [Create a Launch Template for the Auto Scaling Group](2.7-createlaunchtemplate/)

5.  **Set Up Load Balancing**
    *   [Create a Target Group](2.8-createtargetgroup/)
    *   [Create an Application Load Balancer (ALB)](2.9-createalb/)

6.  **Configure Auto Scaling**
    *   [Create an Auto Scaling Group (ASG)](2.10-createasg/)

7.  **Storage and Content Delivery (CDN)**
    *   [Create an S3 Bucket for the Frontend](2.11-creates3/)
    *   [Create CloudFront for the Backend (pointing to ALB)](2.12-createcloudfrontbe/)
    *   [Create CloudFront for the Frontend (pointing to S3)](2.13-createcloudfrontfe/)