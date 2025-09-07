---
title : "Deploying an Online Restaurant Project on AWS"
date : "2025-09-06" 
weight : 1 
chapter : false
---
# Deploying a Full-Stack Online Restaurant Project on AWS

### Overview

In this workshop, you will be guided step-by-step to deploy a complete online restaurant application on the Amazon Web Services (AWS) platform. The project includes a frontend built with HTML, CSS, and JavaScript, along with a powerful backend using Node.js (Express) and a MySQL database (RDS), creating a scalable, secure, and high-performance system.

![Project Architecture on AWS](/images/ors-demo.png) 

### Technologies and Services Used

ℹ️ The goal of the workshop is to build a professional cloud infrastructure that is fault-tolerant and auto-scaling to meet user traffic demands.

💡 **The main AWS services used are:**

*   **VPC (Virtual Private Cloud):** Create a separate and secure network environment on AWS, including Public Subnets for resources that need internet access and Private Subnets to protect the database.
*   **EC2 (Elastic Compute Cloud):** Where the Node.js backend application is deployed and run.
*   **AMI & Launch Templates:** Create a "template" for the server, helping to automate the creation of new EC2 instances uniformly.
*   **Application Load Balancer (ALB):** Distribute traffic to multiple EC2 instances, increasing the application's availability and load-handling capacity.
*   **Auto Scaling Group (ASG):** Automatically adjust the number of EC2 instances based on traffic, ensuring stable performance and cost optimization.
*   **RDS (Relational Database Service):** Provide a fully managed MySQL database, simplifying installation, operation, and backup.
*   **S3 (Simple Storage Service):** Store and serve the frontend's static resources (HTML, CSS, JavaScript, images).
*   **CloudFront:** A Content Delivery Network (CDN) service that helps speed up page load times for users globally and secures both the frontend and backend.

### Architecture and Scope

ℹ️ The project is clearly separated into two parts: **Frontend** (user interface) and **Backend** (processing system), deployed independently but communicating closely with each other via API.

**Backend (Server-side)**

*   The Node.js application is deployed on **EC2 instances** within an **Auto Scaling Group**.
*   The **Application Load Balancer** will receive requests from users and forward them to the active EC2 instances.
*   The **RDS MySQL** database is placed in a Private Subnet, only allowing access from EC2 instances to ensure maximum security.
*   A **CloudFront distribution** is configured to point to the ALB, providing a layer of protection and acceleration for the API endpoints.

🔒 **Frontend (Client-side)**

*   The entire frontend source code (static files) is uploaded to an **S3 bucket**.
*   This S3 bucket is configured to function as a static website.
*   Another **CloudFront distribution** is set up to serve content from the S3 bucket. The use of **Origin Access Identity (OAI)** ensures that users can only access the frontend through CloudFront, preventing direct access to the S3 bucket.

💡 **Let's start building a powerful and professional infrastructure for your application in the following sections!**