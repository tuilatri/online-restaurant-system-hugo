---
title : "Connecting and Deploying the Application"
date : "2025-09-06" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

After having built the entire infrastructure on AWS, the next step is to connect to the EC2 instance, install the necessary environment, and deploy the backend application's source code.

Since our initial `ors-ec2` server is placed in a **Public Subnet** and has been assigned a public IP, we can connect directly to it from our personal computer using SSH to perform the setup tasks.

The process in this chapter will include:
1.  **Connecting** to the EC2 instance using the created `.pem` file.
2.  **Installing** necessary tools like Git, Node.js.
3.  **Downloading** the project's source code from GitHub to the server.
4.  **Configuring** environment variables to connect to the RDS database.
5.  **Running** the backend application and testing its operation.

{{% notice info %}}
In this chapter, **MobaXterm** will be the primary SSH client tool used on Windows to make connections and run commands. You can also use other tools like Terminal (macOS/Linux) or PuTTY.
{{% /notice %}}