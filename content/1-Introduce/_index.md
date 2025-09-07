---
title : "Introduction and Architecture Overview"
date : "2025-09-02" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

### Architecture Overview

In this workshop, we will deploy a complete weather forecast application onto the AWS environment. The architecture is designed to ensure High Availability, Scalability, and Security by using core AWS services.

Below is the overall architecture diagram of the project we will be building:

![Project Architecture](/images/1.introduction/weather-application-architecture.png) 

### AWS Services Used

Here is a list of the main services and their roles in this project:

#### Networking and Security

*   **Amazon VPC (Virtual Private Cloud)**: This is the networking foundation for the entire project. We use a VPC to create a separate and isolated network environment on AWS, allowing us to have complete control over network resources, including IP address ranges, subnets, and route tables.

*   **Public & Private Subnets**:
    *   **Public Subnets**: Used for resources that need direct access from the Internet, such as the Application Load Balancer and the Bastion Host.
    *   **Private Subnets**: Used to host sensitive resources like the backend EC2 instances, preventing direct access from the Internet to enhance security.

*   **Security Groups**: They act as a virtual firewall for EC2 instances to control inbound and outbound traffic. In this project, we configure separate Security Groups for the Bastion Host, the Backend servers, and the Application Load Balancer to only allow necessary connections.

#### Compute and Scaling

*   **Amazon EC2 (Elastic Compute Cloud)**: Provides virtual servers to run the application.
    *   **Backend Instances**: EC2 instances located in the private subnet, responsible for running the Node.js application to handle logic and communicate with the weather API.
    *   **Bastion Host**: An EC2 instance placed in the public subnet, acting as a secure access gateway for administrators to SSH into and manage the backend servers in the private subnet.

*   **Application Load Balancer (ALB)**: Automatically distributes incoming traffic from users to multiple backend EC2 instances. The ALB helps increase the application's availability and fault tolerance. It also performs health checks to ensure requests are only sent to healthy servers.

*   **Auto Scaling Group (ASG)**: Automatically adjusts the number of backend EC2 instances based on the actual load. When traffic increases, the ASG automatically adds new servers and removes them when traffic decreases, helping to optimize costs and ensure performance.

#### Storage and Content Delivery

*   **Amazon S3 (Simple Storage Service)**: A highly scalable object storage service. We use S3 to store the entire frontend source code (HTML, CSS, JavaScript) and configure it to function as a static website.

*   **Amazon CloudFront**: A content delivery network (CDN) service that helps accelerate the loading of web pages and APIs for users worldwide.
    *   **Frontend Distribution**: Distributes static content from S3, caching it at Edge Locations closest to the users, while also securing the S3 bucket using Origin Access Identity (OAI).
    *   **Backend Distribution**: Provides a secure HTTPS endpoint for the API, forwarding requests to the Application Load Balancer.