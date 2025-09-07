---
title : "Conclusion and Next Steps"
date : "2025-09-04" 
weight : 5
chapter : false
pre : " <b> 5. </b> "
---

**Congratulations on completing the workshop!**

You have successfully built a modern, scalable, and highly available web application architecture on the AWS platform. This is not just a simple exercise but a solid foundation that simulates how real-world systems are constructed in the cloud.

Instead of just looking back at what has been done, let's explore how to make this system even better, more automated, and more robust.

### Upgrading and Extending the Project
The current architecture is an excellent starting point. Below are potential improvement paths you can explore to enhance your skills:

#### **1. Automate Deployment with a CI/CD Pipeline**
*   **Problem:** Currently, the code update process is manual (SSH into the server, `git pull`, `npm restart`). This process is time-consuming, prone to errors, and unsuitable for a professional environment.
*   **Solution:** Build a CI/CD (Continuous Integration/Continuous Deployment) pipeline using services like **AWS CodePipeline**, **AWS CodeBuild**, and **AWS CodeDeploy**. With CI/CD, every time you push new code to GitHub, the system will automatically build, test, and deploy the new version to the EC2 instances without any manual intervention.

#### **2. Manage Infrastructure with Code (Infrastructure as Code - IaC)**
*   **Problem:** Setting up the entire infrastructure manually via the AWS Console (ClickOps) is difficult to reproduce accurately and lacks version control capabilities.
*   **Solution:** Learn to use IaC tools like **AWS CloudFormation** or **Terraform**. You can define the entire architecture (VPC, EC2, ALB, S3, etc.) in code files. This allows you to create, update, or delete the entire environment with just a few commands, ensuring consistency and automation.

#### **3. Integrate a Managed Database**
*   **Problem:** Our application currently lacks a database for persistent data storage.
*   **Solution:** Instead of manually installing a database on an EC2 instance (which requires significant administrative effort), use AWS's managed database services.
    *   **Amazon RDS (Relational Database Service):** For relational databases like MySQL, PostgreSQL.
    *   **Amazon DynamoDB:** For NoSQL databases, suitable for applications requiring high performance and low latency.

#### **4. Use a Custom Domain with Amazon Route 53**
*   **Problem:** Users are accessing the application through the default, long, and hard-to-remember CloudFront domain names.
*   **Solution:** Use **Amazon Route 53**, AWS's DNS service, to register a custom domain (e.g., `my-weather-app.com`) and point it to your CloudFront distributions.

#### **5. Monitoring, Logging, and Alerting with Amazon CloudWatch**
*   **Problem:** How do you know if the application is running smoothly or encountering errors? When should Auto Scaling be triggered?
*   **Solution:** Integrate more deeply with **Amazon CloudWatch**. You can:
    *   **CloudWatch Logs:** Collect logs from your applications on EC2 for debugging.
    *   **CloudWatch Metrics:** Monitor key performance indicators like CPU Utilization and Request Count.
    *   **CloudWatch Alarms:** Set up automated alerts that send you emails or messages when an issue occurs (e.g., high CPU, website is down).

#### **6. Enhance Security with AWS WAF**
*   **Problem:** The application could be vulnerable to common web attacks such as SQL injection or cross-site scripting (XSS).
*   **Solution:** Integrate **AWS WAF (Web Application Firewall)** with your CloudFront distribution or Application Load Balancer. WAF helps protect your application by filtering and blocking malicious traffic.

Your journey with AWS has just begun. With the solid foundation from this workshop, you are now ready to explore and conquer more complex services and challenges.

**Happy building!**